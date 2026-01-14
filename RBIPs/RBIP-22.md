## Preamble

    RBIP: 22
    Title: Cross-chain adapter
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2023-09-02

## Simple Summary

Allow pools to perform cross-chain token bridging transactions.

## Abstract

Rigoblock pools perform cross-chain transactions by means of a custom extension that allowes interacting with pre-set bridges and batches approval settings in the transaction.

## Motivation

In general, tokens will have different levels of liquidity and different liquidity-providing APRs on different chains, which will open opportunities for pools generating yields.
Also, some tokens might be available on a few chains only. Given the possibility of creating arbitrage strategies across chains, it is desirable to be able to transfer tokens
from 1 supported chain to another within the same pool.


## Specification

A cross-chain bridge adapter should be developed. Intents are a smart way to express a desired output on a specific chain while transferring a specific token on the origin chain.
Across in an intent-based bridge, that supports attaching custom data to the spoke on the origin chain, which the solver will have to pass to the spoke on the destination chain.
This allows creating virtual token balances that result in an unchanged unitary value on both chains.


## Notes


## Test Cases


## Implementation
A sample implementation:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@across-protocol/contracts/interfaces/SpokePoolInterface.sol";

// Interface for Across Bridge SpokePool (V3, simplified)
interface ISpokePool {
    function depositV3(
        address depositor,
        address recipient,
        address inputToken,
        address outputToken,
        uint256 inputAmount,
        uint256 outputAmount,
        uint256 destinationChainId,
        address exclusiveRelayer,
        uint32 quoteTimestamp,
        uint32 fillDeadline,
        uint32 exclusivityDeadline,
        bytes calldata message
    ) external payable;

    function requestDepositRefund(
        uint32 depositId,
        address inputToken,
        uint256 inputAmount,
        address depositor
    ) external;
}

// Stateless contract, runs in vault's context via delegatecall
contract RigoblockVaultCrossChainExtension {
    // Immutable addresses (set by vault during deployment)
    address public immutable spokePool;
    address public immutable wethAddress;

    // Storage slots (defined in vault, accessed via delegatecall)
    // mapping(address => int256) public virtualBalances; // Cumulative virtual balance per token
    // mapping(uint256 => mapping(address => address)) public tokenAddressMap;
    // mapping(bytes32 => bool) public transferRequests;

    // Events (emitted by vault via delegatecall)
    event TransferRequested(
        bytes32 indexed requestId,
        address indexed inputToken,
        address indexed outputToken,
        uint256 inputAmount,
        uint256 outputAmount,
        uint256 destinationChainId
    );
    event TransferCompleted(
        bytes32 indexed requestId,
        address indexed outputToken,
        uint256 outputAmount
    );
    event RefundRequested(
        bytes32 indexed requestId,
        address indexed inputToken,
        uint256 inputAmount
    );

    constructor(address _spokePool, address _wethAddress) {
        spokePool = _spokePool;
        wethAddress = _wethAddress;
    }

    // Operator requests a cross-chain transfer (called by vault via delegatecall)
    function requestTransfer(
        address inputToken,
        address outputToken,
        uint256 inputAmount,
        uint256 outputAmount,
        uint256 destinationChainId,
        uint32 quoteTimestamp,
        uint32 fillDeadline,
        uint32 exclusivityDeadline
    ) external payable returns (bytes32 requestId) {
        // Check vault balance
        bool isNative = inputToken == address(0);
        address tokenToUse = isNative ? wethAddress : inputToken;

        // Validate token pair (vault storage)
        require(
            getVaultTokenMapping(destinationChainId, inputToken) == outputToken,
            "Invalid outputToken"
        );

        // Generate requestId and mark as pending
        requestId = keccak256(abi.encodePacked(inputToken, outputToken, inputAmount, outputAmount, destinationChainId, block.timestamp));
        setVaultTransferRequest(requestId, true);

        // Add inputAmount to virtual balance on origin chain (cumulative)
        setVaultVirtualBalance(inputToken, int256(inputAmount));

        // Approve SpokePool to transfer ERC20 tokens (not needed for WETH with msg.value)
        if (!isNative) {
            IERC20(inputToken).approve(spokePool, inputAmount);
        }

        outputAmount = outputAmount < inputAmount * 999 / 1000 ? inputAmount * 999 / 1000 : outputAmount;

        // Initiate transfer via Across Bridge (vault context)
        ISpokePool(spokePool).depositV3Now{value: isNative ? inputAmount : 0}(
            address(this), // depositor (vault)
            address(this), // recipient (vault on destination chain)
            inputToken,
            outputToken,
            inputAmount,
            outputAmount,
            destinationChainId,
            address(0), // exclusiveRelayer (any solver)
            fillDeadlineOffset,
            exclusivityParameter,
            abi.encode(requestId, outputToken, outputAmount)
        );

        emit TransferRequested(requestId, inputToken, outputToken, inputAmount, outputAmount, destinationChainId);
    }

    // Handle incoming transfer on destination chain (called by SpokePool)
    function handleV3AcrossMessage(
        address tokenReceived,
        uint256 amount,
        address relayer,
        bytes memory message
    ) external {
        // this check could be unnecessary as the vault will verify the caller is the spoke before giving write access to this call
        require(msg.sender == address(spokePool), "Only SpokePool can call");

        // Decode message
        (bytes32 requestId, address outputToken, uint256 expectedAmount) = abi.decode(
            message,
            (bytes32, address, uint256)
        );

        // Validate token and amount
        require(tokenReceived == outputToken, "Invalid token received");
        require(amount == expectedAmount, "Invalid amount received");

        // Subtract amount from virtual balance on destination chain (cumulative)
        setVaultVirtualBalance(outputToken, -int256(amount));

        // Tokens are automatically sent to the vault (this contract's address via delegatecall)
        emit TransferCompleted(requestId, outputToken, amount);
    }

    // Operator requests a refund for an unfilled order
    function requestRefund(
        uint32 depositId,
        bytes32 requestId,
    ) external {
        // Call SpokePool to refund tokens
        ISpokePool(spokePool).requestDepositRefund(depositId, inputToken, inputAmount, address(this));

        // Subtract inputAmount from virtual balance (cumulative)
        setVaultVirtualBalance(inputToken, -int256(inputAmount));

        // Clear transfer request
        setVaultTransferRequest(requestId, false);

        emit RefundRequested(requestId, inputToken, inputAmount);
    }

    // Internal function to update vault's virtualBalances (additive)
    function setVaultVirtualBalance(address token, int256 amount) internal {
        // Add/subtract amount to/from existing virtual balance (cumulative)
        bytes32 slot = keccak256(abi.encodePacked("virtualBalances", token));
        int256 currentBalance;
        assembly {
            currentBalance := sload(slot)
        }
        int256 newBalance = currentBalance + amount; // Additive update
        assembly {
            sstore(slot, newBalance)
        }
    }

    function vaultHasTokenMapping(uint256 chainId, address token) internal view returns (bool) {
        bytes32 slot = keccak256(abi.encodePacked("tokenAddressMap", chainId, token));
        address mappedAddress;
        assembly {
            mappedAddress := sload(slot)
        }
        return mappedAddress != address(0);
    }

    function getVaultTokenMapping(uint256 chainId, address token) internal view returns (address) {
        bytes32 slot = keccak256(abi.encodePacked("tokenAddressMap", chainId, token));
        address mappedAddress;
        assembly {
            mappedAddress := sload(slot)
        }
        return mappedAddress;
    }

    function vaultHasTransferRequest(bytes32 requestId) internal view returns (bool) {
        bytes32 slot = keccak256(abi.encodePacked("transferRequests", requestId));
        bool status;
        assembly {
            status := sload(slot)
        }
        return status;
    }

    function setVaultTransferRequest(bytes32 requestId, bool status) internal {
        bytes32 slot = keccak256(abi.encodePacked("transferRequests", requestId));
        assembly {
            sstore(slot, status)
        }
    }

    // Receive ETH for native token transfers
    receive() external payable {}
}
```

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

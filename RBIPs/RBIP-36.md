## Preamble

    RBIP: 36
    Title: Withdraw Pool-owned Tokens
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
    Created: 2024-03-11

## Simple Summary

To allow a smart pool holder to burn pool tokens and receive an pool-owned token other than the base token.


## Abstract

A burn transaction can be successfully executed even when the pool does not hold enough base tokens as the system transfers an equivalent amount of one or many other pool-held tokens.

## Motivation

The feature is very important as it allows a user to withdraw/burn tokens even when a smart pool does not hold enough balance. This prevents abuse from the pool operator, as the holder can choose whether to withdraw an amount of base token or an amount of the smart pool's owned token/contract position.
This feature is conditional to the implementation of [RBIP-35](https://github.com/RigoBlock/RBIPs/issues/35).

## Specification

A new method is added to the *Actions* sub-contract
```
function withdrawWithTokens(uint256 amountIn, uint256 amountOutMin, address contract, bytes32 positionId)
```
where positionId is an optional parameter, used when a position is withdrawn.
The feature could initially only allow withdrawing owned tokens, and therefore be implemented as
```
function withdrawWithTokens(uint256 amountIn, uint256 amountOutMin, address tokenAddress)
```

A holder will have the ability to select a token to withdraw, how many smart pool tokens to burn and a minimum of received tokens. The protocol will evaluate the position through the oracles extension and apply the spread/markup. The last holder will not be applied a markup.
Sufficient tests should be developed, to assert the system cannot be abused. As a general rule, when implementing this feature, the default smart pool holding period in the implementation should be set to 1 day (currently 1 block).

This feature could be implemented without modifications to the interface in the `burn` method by automatically sending another token if the base token balance is not enough to cover the withdrawal request. In this case, the attack surface is smaller, as any attacker would have to wait for the pool operator to allocate previously received base tokens to manipulate the oracle, which would make the attack ineffective. In this case, the protocol would rank owned assets by ETH value, and start withdrawing from a bigger balance first. This way, an attacker could not select the asset to withdraw, but the system would choose automatically.

## Notes
As with RBIP-35, we'd prefer this feature be implemented as a library rather than as an extension, so be part of the core instead of being a moving component. To reduce the size of the implementation, the method should be added to the *Actions* sub-contract, with a direct call (no forwarding to self, which would otherwise execute as view since write calls to the extensions are restricted to the pool owner) to the custom extension. In this context, we would like to prevent a whitelister from updating the selector mapping and restrict this permission to the governance only.

This feature will substitute [RBIP-26](https://github.com/RigoBlock/RBIPs/issues/26).

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

## Preamble

    RBIP: 35
    Title: Autonomous Unitary Value Calculation
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Final
    Created: 2024-03-08

## Simple Summary

This proposal automates a smart pool's value and unitary value estimate in a decentralized and fully autonomous way.


## Abstract

The process of calculating and updating a smart pool's unitary value (used for mint/burn amounts calculations) is now decentralized from the core to the pool operator. The introduction of advanced Geomean Oracles in Uniswap V4 opens the way for implementing a completely autonomous unitary value calculation.

## Motivation

The process of updating a pool's unitary value is currently not automated (even though it can be at the smart pool operator's level), not trustless, and not in real-time (as the update requires storing the new value on-chain). Current existing solutions are centralized (i.e., ChainLink and similar), subject to oracle manipulation, and not in real-time (oracles are updated only periodically, i.e., once every 1 hour). The resulting unitary value would be only an estimate and would require further safety measures at the smart pool level to prevent abuse. Furthermore, centralized on-chain Oracle providers do not offer a censorship-resistant system, as they arbitrarily decide what tokens they do support and regularly discontinue support for some tokens.
Uniswap V4, on the other hand, provides a standard for on-chain settlement and a standard for on-chain Oracles. Their Geomean Oracles can be used to estimate unitary value in a precise and accurate way.

## Specification

A new extension *EUnitaryValue.sol* is added to the protocol, which makes a query to the Uniswap V4 Oracle liquidity pool. To find the correct Oracle, the extension will store the Uniswap V4 *Oracle hook* address, which is the same on all networks, in its bytecode as an `immutable`. It will then compute the correct token to ETH price by calling `function observe(PoolKey calldata key, uint32[] calldata secondsAgos)` in two distinct points in time, and then dividing the difference of `tickCumulatives` by the difference of `secondsPerLiquidityCumulativeX128s`.
PoolKey is defined as follows:
```
/// @notice Returns the key for identifying a pool
struct PoolKey {
    /// @notice The lower currency of the pool, sorted numerically
    Currency currency0;
    /// @notice The higher currency of the pool, sorted numerically
    Currency currency1;
    /// @notice The pool swap fee, capped at 1_000_000. The upper 4 bits determine if the hook sets any fees.
    uint24 fee;
    /// @notice Ticks that involve positions must be a multiple of tick spacing
    int24 tickSpacing;
    /// @notice The hooks of the pool
    IHooks hooks;
}
```
Because in Uniswap V4 only one oracle pool is allowed, the inputs are:
```
PoolKey (
    currency0: address(0),
    currency1: tokenAddress,
    fee: 0,
    tickSpacing: type(int16).max,
    hooks: Oracle
```
owned token balances are multiplied by price and summed, then converted to the base token by querying the base token to ETH price.

Pool positions are evaluated from the positions contract, where we either retrieve the aggregate ETH value of positions or extract the individual token components of the positions and sum (subtract in case of debt position) to the pool's token balances.
**Notice: _when a token balance is sold, the token should be removed from the mapping of tracked tokens. However, if a position owns the same token, we must assert that the token balance can be evaluated when the position is closed. Furthermore, services like DefiLlama and Zapper use the list of owned tokens to evaluate tokens in positions. Technically, it would be ideal to decompose the positions in the underlying tokens and remove a token from the mapping when its balance is null. We also must check what happens in the unrealistic edge scenario where the balance is nill for example due to a positive balance plus an equivalent debt position. In this context, if the token is removed from the mapping, we must ensure that whenever the balance is not null anymore it is correctly tracked._**
As a general rule, we should request that no more than 250 tokens and 250 positions be held at a time. While this is not a strict requirement, we want to ensure that the value calculation cost does not explode as a pool buys and sells tokens.

Alternatively, a new library is added to the protocol and inherited by the smart pool implementation. In this case, any further upgrades to the library code will require a pool implementation upgrade (i.e., governance approval + smart pool operator approval).
**_Ideally we'd want the feature to be implemented as a library to be part of the core instead of the extensions. This will lead to a bigger smart pool implementation contract, which is not huge anyway. While any upgrade to this will require both governance and pool operator approval, it is probably desirable that mint and burn function are not a moving component._**

As a general rule, when implementing this feature, the default smart pool holding period in the implementation should be set to 1 day (currently 1 block) unless tests prove the current default is sufficient to prevent oracle manipulation attacks.

## Notes
This RBIP requires adding a storage slot for owned tokens (plus one for owned positions), as in [RBIP-28](https://github.com/RigoBlock/RBIPs/issues/28).

Oracles should be pruned of front-and-back running rogue price updates (flashbot attacks that can last 2 or more consecutive blocks). Oracle could be truncated to 9116 ticks (2.5x price movement), but a back-running protection should prevent a rogue oracle update regardless. An oracle may be considered valid if if contains a minimum of liquidity, but this is difficult to assert formally as the threshold would be discretionary.
Setting a longer default burn lockup (currently 2 blocks) will also help reduce the attack surface.

Notice: the RBIP is approved and will be included in the Rigoblock V4 SmartPool implementation upgrade (subject to onchain governance upgrade of the canonical implementation in the proxy factory smart contract). Individual smart pools will have to upgrade proxy implementation in order to use this new feature.

## Test Cases
[Smart pool updated tests](https://github.com/RigoBlock/v3-contracts/blob/development/test/core/RigoblockPool.spec.ts)
[Base token smart pool updated tests](https://github.com/RigoBlock/v3-contracts/blob/development/test/core/RigoblockPool.Basetoken.spec.ts)

## Implementation
[Automated nav Github PR](https://github.com/RigoBlock/v3-contracts/pull/622)
[MixinPoolValue.sol](https://github.com/RigoBlock/v3-contracts/blob/development/contracts/protocol/core/state/MixinPoolValue.sol)


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

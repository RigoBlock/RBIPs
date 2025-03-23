## Preamble

    RBIP: 39
    Title: Deprecate Token Whitelist
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Final
    Created: 2024-03-13

## Simple Summary

To allow tokens in the smart pools instead without central control.


## Abstract

To deprecate using a governance-controlled token whitelist and allow any token in the smart pools instead.

## Motivation

The token whitelist has been introduced to easily allow tracking of pool-owner assets externally, and to limit pool operator abuse when a pool would purchase units of a rogue token, or an interaction with an arbitrary external contract could lead to unintended consequences (i.e., unforeseen reentrancy attacks) on the pool. Uniswap v4 sets a standard for on-chain oracles, which could be used to limit those issues by design. To be able to swap to a rogue token a pool operator would first have to:
- deploy a rogue token
- create a Uniswap v4 liquidity pool with a supported oracle hook
- burn the rogue tokens by feeding into the oracle
- extend oracle cardinality and execute the minimum number of swaps to be considered valid oracle by Rigoblock

As these actions express rogue behaviour, a rogue token cannot be accidentally added to a smart pool. It is pointless restricting the token universe as even with very liquid tokens a pool operator can perform a front-and-back running attack to extract value from an operated smart pool.

## Specification

- universal access to potentially any token
- remove governance control over the allowed tokens
- use a new Rigoblock protocol extension
- store owned tokens in the smart pools' storages (should also store pool-owned positions)
- cap the maximum number of tokens to 250
- clear owned token mapping when a token is sold completely
  - this will avoid exploding gas costs in mint and burn operations as a smart pool gets in and out a big number of tokens.

Intended flow:
- on *create pool* operations, the system will check that the chosen base token has an oracle feed as per specifications
  - the transaction will revert if no price feed is found, as otherwise it won't be possible to calculate the pool value in base token units.
- on *swap* operations, the system will check that the incoming token has an oracle price feed as per specifications, and will return an error if it does not.

If an oracle feed for a said token exists, but its cardinality is too low, we should consider extending it as a fallback, instead of reverting the transaction. However, this will add to the transaction cost, so we may instead return an error message on the interface prompting the user to extend the cardinality himself.

The oracle pair will always be *target token* and *Token(address(0))* for universal encoding. We must check if this correctly works on all networks (polygon especially as MATIC has multiple non-null addresses).
A canonical oracle implementation will be selected (either a Uniswap canonical oracles hook, or a custom designed one).
Retrieving the oracle key is simple, as it is a function of known parameters:
- currency0 -> `Currency(address(0))`
- currency1 -> `Currency(targetToken)`
- fee ->  `uint24(0)` (oracles have 0 fee)
- tickSpacing -> `MAX_TICK_SPACING`
- hooks -> `IHooks(canonicalOracleHookAddress)`

It is yet to be decided whether the oracle feed assertions should be developed in a non-upgradable library (any changes will require pool operator's implementation upgrade) or in a dedicated extension. As the code relevant to assertions does not execute write operations, using an extension does not pose security concerns, however will increase the gas cost by ~2100 gas. Pros and cons of using an extension within the core contracts (i.e., *mint* and *burn* operations) will be evaluated.

Notice: the price feed check is executed only if the token is not already active, i.e. first the expected storage slot is read (2100 gas) and then the oracle call is performed as a fallback. This makes transactions using frequently used tokens less gas-expensive. The new feature will be included in Rigoblock V4.
 
## Test Cases
[Modify liquidity tests](https://github.com/RigoBlock/v3-contracts/blob/f291f12a8b3f07b8893038551177db030eba295c/test/extensions/AUniswapRouter.spec.ts#L221)
[Swap tests](https://github.com/RigoBlock/v3-contracts/blob/f291f12a8b3f07b8893038551177db030eba295c/test/extensions/AUniswapRouter.spec.ts#L911)

## Implementation
[Oracle extension](https://github.com/RigoBlock/v3-contracts/blob/development/contracts/protocol/extensions/EOracle.sol)
[Price feed assertion](https://github.com/RigoBlock/v3-contracts/blob/f291f12a8b3f07b8893038551177db030eba295c/contracts/protocol/libraries/EnumerableSet.sol#L62)
[Oracle use in token operation](https://github.com/RigoBlock/v3-contracts/blob/f291f12a8b3f07b8893038551177db030eba295c/contracts/protocol/extensions/adapters/AUniswapRouter.sol#L197)


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

## Preamble

    RBIP: 39
    Title: Deprecate Token Whitelist
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
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

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

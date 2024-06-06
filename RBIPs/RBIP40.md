## Preamble

    RBIP: 40
    Title: Introduce max slippage is slippage-sensitive operations
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
    Created: 2024-06-06

## Simple Summary

To assert that a swap does not get executed beyond a certain delta from a recent price.


## Abstract

In order to prevent frontrunning attacks, on top of the maximum slippage set at a client level, we can implement slippage protection against an oracle at a protocol level.

## Motivation

If a client sets a wrong uint256 minimumAmountOut parameter in a swap call, the call can be MEV-arbitraged.
To prevent the wrong parameter set at an interface level, we can add a small gas cost increase for a swap execution and add a check of maximum delta against an oracle price, say 9116 ticks (~2.5x oracle price) just as an example.
The same can be applied to liquidity positions.

## Specification
TBD. Depends on onchain oracles being implemented.

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

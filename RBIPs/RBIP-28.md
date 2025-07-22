## Preamble

    RBIP: 28
    Title: Allocate storage slot for pool owned tokens
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2024-03-03

## Simple Summary

Assign a storage slot to pool owned tokens.

## Abstract

Before v4, pool owned tokens were tracked from the whitelist. With the intruction of v4, we have the ability to track the owned tokens only.

## Motivation

Gas efficiencies in nav calculations.


## Specification

Assign a deterministic storage slot to owned tokens.


## Test Cases

Covered by the v4 test cases.

## Implementation

https://github.com/RigoBlock/v3-contracts/blob/3c13e1560a0d0ada09309d4fbef2d689976c512e/contracts/protocol/core/immutable/MixinConstants.sol#L41

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

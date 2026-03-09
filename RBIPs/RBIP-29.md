## Preamble

    RBIP: 29
    Title: DEX aggregator adapter
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Final
    Created: 2026-03-09

## Simple Summary

Implement a DEX aggregator adapter for allowing pools to swap to source liquidity from multiple sources at the same time.


## Abstract

Implement 0x aggregator's 0xSettler via AllowanceHolder.

## Motivation

To pull liquidity from multiple liquidity sources to offer users deep liquidity for swaps.

## Specification

- implement 0x aggregator adapter
- the adapter implements:
   - set infinite allowance before swap
   - assert target token has a price feed
   - remove allowance after swap without clearing storage for efficiency.
- add adapter to authority (requires onchain governance vote)
- whitelist methods

The functionality is supported via an agentic chat that uses the 0x api.

## Test Cases
[PR](https://github.com/RigoBlock/v3-contracts/pull/859).

## Implementation
[PR](https://github.com/RigoBlock/v3-contracts/pull/859).


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

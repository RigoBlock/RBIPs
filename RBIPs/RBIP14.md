## Preamble

    RBIP: 14
    Title: Rigoblock V3
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2022-06-02

## Simple Summary

Deploy a lightweight, upgradable V3 protocol.

## Abstract

Develop and deploy a new protocol that allows for inexpensive pool creation and efficient operations.

## Motivation

New techniques allow for lightweight, proxy-based smart pools. These can save 95% of the deployment cost.
With gas optimizations, operating a pool will not be more gas-expensive than v2 pools.
Deterministic deployment allows for the same pool address on different chains, allowing for cross-chain pools.

## Specification

- proxy-based deployment
- deterministic deployment
- deterministic storage slots
- split implementation code in mixins for easier auditability
- better handling of owner vs user actions

## Notes

## Test Cases

## Implementation

[https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3.sol
](https://github.com/RigoBlock/v3-contracts/tree/development/contracts)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

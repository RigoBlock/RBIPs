## Preamble

    RBIP: 17
    Title: Upgrade Proof of Performance
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2022-07-06

## Simple Summary

Simplify Proof of Performance.

## Abstract

Proof of Performance should be universal across pools with different base tokens and pool-value agnostic.

## Motivation

Rigoblock V3 pools can use any token as base token, therefore it is not possible to evaluate pools with different base tokens
without using an oracle. We want to build an incentives system that is universal and oracle-agnostic.

## Specification

The new proof of performance should use GRG as base, which is universally available on all supported chains and can be used
as a reference measure for all. Instead of evaluating the pool's value and performance directly, we make the indirect assumption
that the bigger the pool, the better the performance, the more GRG it can acquire and stake to itself. Therefore, the pool's
own staked GRG is used as a proxy for its "mining" power in the staking system.

## Notes

[https://github.com/Uniswap/v3-periphery/tree/main/contracts
](https://github.com/RigoBlock/v3-contracts/blob/development/contracts/rigoToken/proofOfPerformance/ProofOfPerformance.sol)

## Test Cases

https://github.com/RigoBlock/v3-contracts/blob/development/test/staking/StakingProxy.Pop.spec.ts

## Implementation

https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3.sol

https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3NPM.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

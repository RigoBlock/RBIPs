## Preamble

    RBIP: 15
    Title: Staking Deployment Constant Upgrade
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2022-09-22

## Simple Summary

Upgrade staking implementation to support Rigoblock V3.

## Abstract

In order to support new Rigoblock pools the staking system requires and upgrade of the staking implementation.

## Motivation

Rigobock V3 uses a new registry which is not upgradable in the staking system.

## Specification

The new staking implementation will use the new registry address as deployment constant input.
The newly deployed implementation must be attached to the staking proxy by first removing the old implementation.

## Notes

## Test Cases

## Implementation

[[https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3.sol
](https://github.com/RigoBlock/v3-contracts/tree/development/contracts)](https://github.com/RigoBlock/v3-contracts/blob/development/contracts/staking/immutable/MixinDeploymentConstants.sol)https://github.com/RigoBlock/v3-contracts/blob/development/contracts/staking/immutable/MixinDeploymentConstants.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

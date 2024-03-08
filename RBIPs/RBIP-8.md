## Preamble

    RBIP: 8
    Title: Staking
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2019-12-04

## Simple Summary

Staking allows pool operators to deploying their full capital to the pools and leveraging on the community of GRG holders.

## Abstract

Pool operator's reward is calculated as an exponentially weighted average of share of proof of performance reward and share of GRG staked to his pool.

## Motivation

GRG staking allows to:
- enable pools operators to deploy their full capital to their strategies
- activating GRG holders, attributing them a key role in the token economics and getting rewarded for that.

## Specification

The new staking system will be based on the 0x project's staking system, but will use the current RigoBlock "Proof of Performance" rewards system, therefore not adding cost to each RigoBlock transaction.

## Test Cases

## Implementation

https://github.com/RigoBlock/rigoblock-monorepo/tree/development/packages/contracts/src/staking

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

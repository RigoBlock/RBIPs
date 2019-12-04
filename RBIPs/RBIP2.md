## Preamble

    RBIP: 19
    Title: reduce incentives abuse by using eth as reference
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2019-08-21

## Simple Summary

Add additional bindings to proof of performance calculation reward.

## Abstract


## Motivation

Proof of performance rewards pools operators based on the value of the pool and, if any, their performance. Given the arbitrary NAV setting by the pool operator in the Drago application, the rewards multiplier for such an application is currently 0. By binding the reward to the Eth in the pool we incentivize the pool operator to maintain sufficient eth in the pool at regular intervals.

## Specification

The proof of performance reward is decreased by the ratio between eth in the pool and the pool value. Each such ratio < 1% entails the reward being zero, as a 1% minimum eth liquidity is required to guarantee NAV integrity.

## Rationale (if a suggestion is proposed)

Dramatically reduce potential abuse of proof of performance algorithm by setting some additional requirements which make the incentives system more decentralized and less vulnerable.

## Notes

ProofOfPerformanceFace.sol needs to be updated as well, in order to include all functions.

## Test Cases

## Implementation

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

## Preamble

    RBIP: 19
    Title: add slashing condition to pop reward based on GRG pool operator balance
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2019-10-15

## Simple Summary

The proposed improvement slashes the reward based on the amount of GRG tokens a pool operator holds. This has the dual effect of making it exponentially difficult to cheat the system and of creating an incentive for holding/accumulating GRGs.

## Abstract


## Motivation

The proposed improvement addresses an edge case where a pool operator can accumulate GRGs by moving ETH from different token pools. By making it exponentially more expensive for the pool operator to accumulate GRGs, we can make the system overall more robust and increase demand for GRG.

## Specification

In the ProofOfPerformance.sol contract, once the pop components are calculated, their sum is adjusted by an exponentially decaying factor according to the formula:
y = (1-decay factor)*k^[(1-decay factor)^(n-1)]
and a decay factor of 18%. The exponential curve is approximated by intervals as follows:

% of total supply multiplier
[ 0.05 +++ [ 100%
[ 0.04 0.05 [ 82%
[ 0.03 0.04 [ 20.1%
[ 0.02 0.03 [ 2.9%
[ 0.01 0.02 [ 0.5%
[ 0.00 0.01 [ 0.2%

## Rationale

## Notes

## Test Cases

## Implementation

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

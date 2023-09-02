## Preamble

    RBIP: 22
    Title: Cross-chain adapter
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Draft
    Created: 2023-09-02

## Simple Summary

Allow pools to perform cross-chain token bridging transactions.

## Abstract

Rigoblock pools perform cross-chain transactions by means of a custom extension that allowes interacting with pre-set bridges and batches approval settings in the transaction.

## Motivation

In general, tokens will have different levels of liquidity and different liquidity-providing APRs on different chains, which will open opportunities for pools generating yields.
Also, some tokens might be available on a few chains only. Given the possibility of creating arbitrage strategies across chains, it is desirable to be able to transfer tokens
from 1 supported chain to another within the same pool.


## Specification

A cross-chain bridge adapter should be developed. It should allow interaction with a few selected bridges, or all of them at choice.



## Notes


## Test Cases


## Implementation


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

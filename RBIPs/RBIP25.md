## Preamble

    RBIP: 25
    Title: Empty initialization fix in pool implementation
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Draft
    Created: 2023-11-7

## Simple Summary

Upgrade implementation to prevent accidental empty pool initialization.

## Abstract

Pool creation does not fail when a standard wallet or a smart contract that does not implement decimals() is used as the base token. The proposed improvement addresses this issue.

## Motivation

Pool creation should be allowed only with correct initialization. Otherwise, the name is reserved in the registry and the pool cannot be used.
This is relevant for users trying to deploy a pool with the same name and address on multiple chains.

## Specification

The call `IERC20(initParams.baseToken).decimals()` will fail silently if the target address does not implement the method.
Therefore, the following part of the code will be skipped, which is the pool initialization.
In order to guarantee that the pool is initialized correctly, we have to use a try/catch statement and revert with an error.
It is not important to revert with the message error as the pool proxy `initializePool()` call will fail with POOL_INITIALIZATION_FAILED_ERROR
without catching the revert reason for gas optimizations.

The decimals initialization should not make an assertion when the token is base currency as we define 18 decimals.
The token assertion should be moved in the try/catch statement and decimals should be defined case by case instead of initially set to 18 and overwritten if different.

## Notes

Requires governance approval. The issue does not affect existing pools, so there is no need for pools to be upgraded by their pool operators to the latest implementation.

## Test Cases

[Tests](https://github.com/RigoBlock/v3-contracts/blob/ba1af6ad43d309efd3ef7e996d646bbd1af8bfa1/test/factory/RigoblockPool.ProxyFactory.spec.ts)

## Implementation

[Proposed implementation](https://github.com/RigoBlock/v3-contracts/pull/368)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

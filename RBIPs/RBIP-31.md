## Preamble

    RBIP: 31
    Title: Deprecate self custody feature
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Extensions
    Status: Final
    Created: 2023-11-26

## Simple Summary

Remove the unused self-custody feature.

## Abstract

Governance will remove the unused self-custody feature.

## Motivation

The feature was added to provide further flexibility to professional market makers. As the feature never got used and breaks the condition that the pool is the owner of all owned tokens,
it does not make sense to keep it. It also adds a point of attack where if the wallet of the pool operator is hacked and the conditions for using the feature are fulfilled,
tokens can be transferred out of the pool.

## Specification

- Remove mapping of self-custody selectors to adapter contract.
- Remove the self-custody adapter in the authority smart contract.
- Remove self-custody-related code (smart contract, deploy, test) from the v3-contracts repository.


## Notes

Requires governance approval. The issue does not affect existing pools, so there is no need for pools to be upgraded by their pool operators to the latest implementation.

## Test Cases

[Tests](https://github.com/RigoBlock/v3-contracts/blob/ba1af6ad43d309efd3ef7e996d646bbd1af8bfa1/test/factory/RigoblockPool.ProxyFactory.spec.ts)

## Implementation

[Proposed implementation](https://github.com/RigoBlock/v3-contracts/pull/368)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

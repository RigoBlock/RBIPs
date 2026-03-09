## Preamble

    RBIP: 54
    Title: Agentic Delegations
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Final
    Created: 2026-03-09

## Simple Summary

Allow delegations to AI agents.


## Abstract

Implement fine-grained delegated permissions to enable delegating specific and potentially partially overlapping access to adapter methods to AI agents.

## Motivation

AI agents are a revolutionary way to manage portfolios, but granular permissions are required for them to seamlessly operate some of the vault's functionalities. EIP-7702 can be implemented by the user regardless, but lacks generic support from wallets, and EIP-7715 permissions are very limited and binding at the moment. Therefore, it is necessary to offer native account abstraction (AA) in the protocol.

## Specification

https://github.com/RigoBlock/v3-contracts/pull/870/changes

## Test Cases
[PR](https://github.com/RigoBlock/v3-contracts/pull/870).

## Implementation
[PR](https://github.com/RigoBlock/v3-contracts/pull/870).


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

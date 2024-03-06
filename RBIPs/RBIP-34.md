## Preamble

    RBIP: 34
    Title: Proxy initialization reentrancy fix
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Governance, Protocol
    Status: Draft
    Created: 2024-03-06

## Simple Summary

Proxy initialization can be reentered. This RBIP addresses this issue.


## Abstract

During proxy deployment (both governance and smart pool) the method `initialized` is called by the proxy constructor. In this case, the wallet trying to deploy the proxy can potentially
reenter initialization as in both governance and protocol an arbitrary external contract is called.

## Motivation

To assert the side effects of a reentrancy and eventually fix it. To prevent inintialization with incorrect parameters which could potentially create a denalial of service attack on the factories
or on the smart pool registry.

## Specification

A non-reentrant modifier should be added to the proxy factory contracts to avoid potential side effects. Since this requires upgrading the factories, we could alternatively apply the modifier
to the initialize method in the implementations, where the external contract is called. The new modifier should use EIP-1553 - transient storage.

Should also check if we want to just use a low-level staticcall when calling an untrusted external contract to read data, as during the initialization. A non-reentrancy modifier removes
the need for gas limit on staticcall to prevent reentrancy.

## Notes
TBD

## Test Cases
TBD

## Implementation
TBD


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

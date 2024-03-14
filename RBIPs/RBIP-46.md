## Preamble

    RBIP: 46
    Title: Represent Pool Proxy as ERC4626
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
    Created: 2024-03-14

## Simple Summary

To expose ERC4626 public methods in the Rigoblock smart pools.


## Abstract

Pool Proxy inherits ERC4626 public methods and implements them according to Rigoblock specs.

## Motivation

ERC2646 provides a standard for vaults that hold tokens. While it is designed for interest-bearing vaults using one token, we could amend the Rigoblock Pool Proxy API to allow easier tracking by blockchain scanners, and possibly stats platforms. Not a top priority as its impact is limited. An interesting advantage is that we could use pre-audited code, therefore reducing the attack surface at the core protocol level.

## Specification

TBD.

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

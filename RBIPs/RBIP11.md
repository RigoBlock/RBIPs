## Preamble

    RBIP: 11
    Title: Improve Self Custody
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2021-01-11

## Simple Summary

Improves existing self custody adapter.

## Abstract

Implements safeTransfer when tokens are transferred through the proxy contract.

## Motivation

Certain old tokens do not return a boolean when being transferred, therefore resulting in transfer fail when called with standard ERC20 transfer call.

## Specification

Use openzeppelin safeTransfer library

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

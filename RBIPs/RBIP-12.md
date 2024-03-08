## Preamble

    RBIP: 11
    Title: Use safeApprove in Uniswap adapter
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2021-01-11

## Simple Summary

Implement safeApprove for token approvals in AUniswapV2.

## Abstract

Implements safeApprove in the Uniswap V2 adapter.

## Motivation

Certain old tokens do not return a boolean, therefore resulting in failed approve when using a standard ERC20 interface.

## Specification

Use OpenZeppeling safeApprove.

## Test Cases

## Implementation

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

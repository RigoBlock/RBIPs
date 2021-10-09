## Preamble

    RBIP: 11
    Title: Improve Self Custody
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2021-01-11

## Simple Summary

Easier to understand GRG holding requirement and transfer any token.

## Abstract

Removes free tier of GRG holding when transferring tokens to self custody. Implements safeTransfer when tokens are transferred using the ASelfCustody adapter.

## Motivation

Self custody is a feature designed for professional market marker who have no issue holding a minimum amount of GRG. Certain old tokens do not return a boolean when being transferred, therefore resulting in transfer fail when called with standard ERC20 transfer call.

## Specification

Amend GRG holding calculations to support new tiers. Use openzeppelin safeTransfer library.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

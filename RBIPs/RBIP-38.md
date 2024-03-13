## Preamble

    RBIP: 38
    Title: Revert with error on burn with base token balance revert
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
    Created: 2024-03-13

## Simple Summary

Revert with an error instead of simple revert when smart pool does not hold enough base token for burn.


## Abstract

Add a returned error when a burn transaction fails because the pool does not hold enough base tokens to pay expected revenue.

## Motivation

Currently, this edge case is handled at a UI level where a user is prevented from burning smart pool tokens when the smart pool does not hold enough base tokens. In this case, the transaction would fail in the underlying token transfer, without a revert error.
By accepting a slightly higher gas cost, since we have to access the underlying token to verify whether the smart pool holds enough balance, we can throw a revert within the burn method. The additional gas cost should only be 100 gas for the balance read, as we need to access the underlying token contract in any case when trying to transfer, plus some computation for calculations.

## Specification

Add one further assertion [before trying to burn](https://github.com/RigoBlock/v3-contracts/blob/6c057497e307e9a5cc3754872abf601b8378bbab/contracts/protocol/core/actions/MixinActions.sol#L74) that `netRevenue` is smaller than new variable `poolTokenBalance`.

## Notes
This RBIP could be made obsolete by either implementing [RBIP-36](https://github.com/RigoBlock/RBIPs/issues/36) and partially by [EBIP-35](https://github.com/RigoBlock/RBIPs/issues/35).

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

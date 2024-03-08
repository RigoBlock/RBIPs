## Preamble

    RBIP: 32
    Title: Refactor safe approve method
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Extensions
    Status: Draft
    Created: 2023-11-26

## Simple Summary

Refactor the safe approve internal method _safeApprove(...args) to avoid the use of low-level calls.


## Abstract

The adapters using safe approve will use a refactored method for better approval error handling. The upgrade will have to be approved by the governance to take effect.

## Motivation

A refactoring of the safe approve method has the scope of:

- avoiding the use of low-level calls
- using a set of more explicit try/catch statements to improve readability
- allowing to exclude edge scenarios without having to make implicit/non-linear assumptions
- excluding the target token not being a smart contract in an alternative way

## Specification

```
function _safeApprove(address token, address spender, uint256 amount) internal {
  try IToken(token).approve(spender, amount) returns (bool success) {
    assert(success);
  } catch {
    try IToken(token).approve(spender, amount) {

    } catch { revert(); }
  }
}
```


## Notes

- An initial implementation has been proposed [here](https://github.com/RigoBlock/v3-contracts/issues/391).
- As Uniswap is launching v4 sometime in Q1 2024, it would be ideal to have this proposal combined with the future proposal to support the new release.

## Test Cases


## Implementation



## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

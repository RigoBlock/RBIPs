## Preamble

    RBIP: 21
    Title: Upgrade Uniswap adapter
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2023-09-01

## Simple Summary

Upgrade the Uniswap protocol adapter for bug fixes and gas optimizations.


## Abstract

This proposal aims at upgrading the current Uniswap protocol extension to allow multi-hop exactOutput swaps.

## Motivation

The current swap adapter is affected by 2 bugs:
  1) unwrapETH method clashing between swap router and npm contracts and
  2) bypass of token whitelist on multi-hop swaps, plus preventing a swap to a whitelisted token when the first pool involves a non-whitelisted token.

These fixes require an upgrade of the Uniswap adapter It has been considered to also upgrade to use the Uniswap universal adapter,
which simplifies the process of approving swap methods. This option, however, has been rejected as the universal router uses pre-encoded transactions which need to be
decoded both in the Rigoblock adapter and in the Uniswap router, therefore not providing gas optimization benefits, while increasing the complexity of the
Rigoblock adapter. This topic will be addressed by a separate RBIP when Uniswap releases its v4 protocol, which will require an adapter
upgrade, in any case, to be supported by Rigoblock.


## Specification

A new Uniswap swap router adapter is to be written and deployed. The new adapter will use the same interface methods as currently, but some
of its methods will be unimplemented. Specifically: unwrap, refund, and sweep token methods will be not implemented. The reason is that 
Rigoblock never uses the Uniswap router as custody for the transaction and, therefore does not need to recover funds sent to the router.

Uniswap defines path as follows:
- path: address[] = [token[0], ..., token[1]) for v2 swaps
- path: bytes = [token[0], fee, token[1]] for v3 single-hop swaps
- path: bytes = [token[0], fee, token[i], fee, token[j], fee, token[1]] for v3 multi-hop swaps

In this context, the whitelisted token assertion for `ExactInput` or `ExactOuput` (multi-hop swap) should follow the path:
```
if path.HasMultiplePools() {
  address tokenOut = path.slice(path.length - ADDR_SIZE, path.length);
}
```
as we know that the token out is the last 20 bytes of the path string.
Defining length = data.length will not necessarily save gas while using the condition `hasMultiplePools` could be levied as this will be used in the multi-hop swap method.

Lastly, the adapter should allow swapping for a non-whitelisted base token, as this would otherwise make it impossible for
pools based on non-whitelisted tokens to swap back to the base token.
```
const tokenOut = ...
if (!tokensOut = pool.baseToken()) { _assertTokenWhitelisted(tokenOut); }
```

## Notes

Updated with gas benefits for swaps that route through multiple pools and support for Uniswap v4.

## Test Cases
https://github.com/RigoBlock/v3-contracts/blob/development/test/extensions/AUniswap.spec.ts

## Implementation
https://github.com/RigoBlock/v3-contracts/blob/development/contracts/protocol/extensions/adapters/AUniswapRouter.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

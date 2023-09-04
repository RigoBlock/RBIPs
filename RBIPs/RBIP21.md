## Preamble

    RBIP: 21
    Title: Upgrade Uniswap adapter
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Draft
    Created: 2023-09-01

## Simple Summary

Upgrade to use uniswap universal router instead of uniswap swap router for swaps.


## Abstract

The Rigoblock governance is a set of smart contracts that formally own all Rigoblock protocol and staking smart contracts.
Smart pools need an extension to interact with the governance.

## Motivation

The current swap adapter is affected by 2 bugs:
  1) unwrapETH method clashing between swap router and npm contracts and
  2) bypass of token whitelist on multi-hop swaps, plus preventing a swap to a whitelisted token when the first pool involves a non-whitelisted token.

These fixes require an upgrade of the Uniswap adapter, but since Uniswap has released a universal adapter, it is desirable to use the universal swap router,
which simplifies the process of approving swap methods.


## Specification

A new Uniswap universal router adapter should be written and deployed. It will support ERC20 tokens only initially.
Technically, we want to see if we can use the current uniswap adapter for npm methods, but since the unwrap method calls the uniswap swap router,
it will still be affected by the inability to unwrap eth when withdrawing liquidity.
In this context, we want to identify whether it is desirable to keep the swap router adapter and the npm adapter separate,
but we clearly need to assert that method selectors do not clash.

The `unwrapETH` call should be always routed to the correct contract, i.e. universal router for swaps, npm for liquidity transactions.

Uniswap defines path as follows:
- path: address[] = [token[0], token[1]) for v2 swaps
- path: bytes = [token[0], fee, token[1]] for v3 single-hop swaps
- path: bytes = [token[0], fee, token[i], fee, token[j], fee, token[1]] for v3 multi-hop swaps

In this context, the whitelisted token assertion for `ExactInput` or `ExactOuput` (multi-hop swap) should follow the path:
```
if path.HasMultiplePools() {
  address tokenOut = path.slice(path.length - ADDR_SIZE, path.length);
}
```
as we know that the token out is the last 20 bytes of the path string.
Defining length = data.length will not necessarily save gas
using the condition `hasMultiplePools` could be levied as this will be used in the multi-hop swap method.

Lastly, the adapter should allow swapping for a non-whitelisted base token, as this would otherwise make it impossible for
pools based in non-whitelisted tokens to swap back to base token. Obviously, we cannot whitelist a token just because
a pool using it as base token exists, therefore it should be implemented as further condition
```
const tokenOut = ...
if (!tokensOut = pool.baseToken()) { _assertTokenWhitelisted(tokenOut); }
```

## Notes

RigoBlock/v3-contracts#307
We must assert that upgrading to uniswap universal router improves gas efficiency. In fact, since the universal router still imports the
v2 and v3 router as the uniswap router2 does, we want to make sure that the added complexity in decoding/encoding universal router is
justified. Otherwise we can fix the issues in the adapter by still using the uniswap router 2 contract and later upgrade to universal router.
One of the positives of upgrading to universal router is that the universal router api will get better support. One of the negatives
is that in late Autumn '23 it will be upgraded again to support uniswap v4, which will entail yet another rigoblock adapter upgrade.

## Test Cases


## Implementation


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

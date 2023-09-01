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

## Notes

RigoBlock/v3-contracts#307

## Test Cases


## Implementation


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

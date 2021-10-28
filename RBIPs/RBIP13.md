## Preamble

    RBIP: 13
    Title: Support Uniswap V3
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2021-10-09

## Simple Summary

Supports Uniswap V3 through 2 new adapters.

## Abstract

Allows performing swaps and providing liquidity to Uniswap V3 on top of existing Uniswap V2 support.

## Motivation

Uniswap V3 consists of a different set of contracts and interfaces than V2, therefore requires a custom adapter implementation.

## Specification

2 new contracts to be added to the protocol:
- AUniswapV3, which supports swaps
- AUniswapNFTMGR, which supports providing liquidity

## Notes

https://github.com/Uniswap/v3-periphery/tree/main/contracts

## Test Cases

## Implementation

https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3.sol

https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3NPM.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

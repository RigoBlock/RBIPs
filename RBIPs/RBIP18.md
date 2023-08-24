## Preamble

    RBIP: 17
    Title: Upgrade Proof of Performance
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2022-07-06

## Simple Summary

Develop protocol staking adapter

## Abstract

Connect the protocol to the GRG staking system.

## Motivation

Smart pools that have their own stake require an adapter to communicate with the staking system.

## Specification

The adapter should allow the pool to stake to itself.
A "stake" method will be implemented which will batch in one transaction:
   1) Set allowance to GRG transfer proxy
   2) stake and deposit to GRG vault
   3) delegate stake to self.
An "undelegateStake" and "unstake" methods will be implemented as well to allow the pool to withdraw its own stake.
A "withdrawDelegatorRewards" method will be developed to allow the pool to withdraw delegator staking rewards while staking.

## Notes

https://github.com/RigoBlock/v3-contracts/blob/development/contracts/protocol/extensions/adapters/AStaking.sol

## Test Cases

https://github.com/RigoBlock/v3-contracts/blob/development/test/extensions/AStaking.spec.ts

## Implementation

https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3.sol

https://github.com/RigoBlock/rigoblock-monorepo/blob/development/packages/contracts/src/protocol/extensions/adapters/AUniswapV3NPM.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

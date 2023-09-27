## Preamble

    RBIP: 24
    Title: Staking implementation improvements
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Draft
    Created: 2023-09-27

## Simple Summary


## Abstract


## Motivation

The moveStake method does not allow moving from one pool to another in 1 single transaction, which it should. Requires staking implementation upgrade in the staking proxy.
In the current staking system, a pool gets initialized in a specific epoch to participate to incentives, then is finalized at the end of the epoch.
This requires two calls and all participating pools must be finalized before the 2nd next epoch. In order to save some gas, the logic could be amended.

## Specification

- query pool data from pool, do not initialize staking pool
- claim reward during the epoch, without credit reward and later finalize
- directly move stake from one pool to another (currently requires deactivating and reactivating in 1 batch transaction, will save some gas)

## Notes

Requires mapping of stake by owner, plus requires storing the sum of each pool's own stake, which gets updated every time a user delegates/undelegates stake.
We need to make sure that we are not using a loop to compute the sum of the pools' own stakes. In a formally correct way, we should read from the Proof of Performance
contract the pop value, however pop would then call the staking proxy to query the pool own stake.
With the new upgrade, rewards distribution will only be distributed entirely (even in the current system the over/under allocations result in an epoch's rewards
to not be entirely distributed, but in this case the amount will likely be much higher) based on how many of the pools are claiming. While with the current implementation
an epoch could potentially be in equilibrium and all rewards would be allocated instead of being rolled to the next epoch, with the proposed upgrade it is very likely
that a big number of rewards will remain unallocated, being rolled over to the next epoch (meaning they will be added to the next epoch's rewards).

## Test Cases


## Implementation


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

## Preamble

    RBIP: 23
    Title: Governance implementation upgrade
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Draft
    Created: 2023-09-13

## Simple Summary

Upgrade the governance implementation by means of an onchain governance voting to support new features.

## Abstract

Rigoblock pools perform cross-chain transactions by means of a custom extension that allows interaction with pre-set bridges and batch approval settings in the transaction.

## Motivation

RPC providers' designs make it impossible to access historical events on some chains. The alleged reason is a degraded performance of nodes,
which indicates that the log feature while saving marginal gas cost on the transaction, will constitute a perpetual burden on the dapp unless its own nodes are run.
Since we are using the proposal logs to retrieve the proposal description, we could store the description in the proposal struct. In order to make this not too much gas-intensive,
It could be ipfs content (even though with ipfs, by design, it is always possible that such description be lost forever).
Notice the length of the description string will change between proposals, which is not a big deal as a string will always use its own storage slot with the length of the string.
Since this requires a governance implementation upgrade in the governance proxy, it is a good opportunity to implement some other minor fixes.


## Specification

- governance quorum upgrade
- assert past failed proposals are not executable by storing quorum in the proposal struct
- add methods in governance
  - add proposer in proposal (will be able to cancel)
  - add cancel method (if proposer does not hold votes)
  - add queue method (should understand if we want this as it's just one extra call)
- store proposal description in the proposal struct
- upgrade Rigoblock governance implementation

Should also fix https://github.com/RigoBlock/v3-contracts/issues/318

L2 ZK governance: do not require staking quorum on L2, run L2s from L1, or from another L2 (this could be split into a separate upgrade for L2s/sidechains, as the timeline is also a bit constrained by the reliability of the L1-L2 messaging bridges.



## Notes
Retrieve proposal description for display even on chains where historical logs are not available, which will also reduce RPC calls on the interface as historical logs for proposals will not need to be queried and kept in sync. The same result will be achieved by implementing a proposals list endpoint and by reading the proposal description from there, which is ok as onchain governance should not experience very frequent proposals.
Currently, a past proposal that has qualified majority but did not reach quorum could theoretically become executable in the future, should the governance decide to lower the quorum below the one reached by such a past proposal. Storing the quorum in the proposal would allow for keeping quorum upgrade implementations as are now, while at the same time guaranteeing that a past failed proposal cannot become executable.
L2 relay-message-base governance would allow to deploy infinitely on each chain. This is especially relevant for L3s. Currently, we are live on 6 networks, and the more networks get added, the more tokens need to be locked in governance, which becomes more difficult as (besides rewards tokens that go entirely to network contributors) total supply is finite. This means that, theoretically, we could end in a situation where a governance takeover on an L2 results in the takeover of the staking proxy and the unclaimed rewards (user's tokens are never at risk as they are stored in the non-upgradable GRG Vault contract), which could lead to a cascade of attacks on the other L2s, ending up in an attack on L1. This is an extremely unrealistic scenario in the current deployments as the thresholds are particularly high, but it implies the ability to support new chains is limited. In order to overcome this, managing the L2 governance from L1 would fix the issue. Obviously, the messaging bridge in this context will have dominance, and in a case like the Multichain takedown, the L2 governance would become unusable. It's a low-priority task as there are pros and cons to evaluate.
Adding new methods to governance will bring it on par with Openzeppelin governance tools.

## Test Cases


## Implementation


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

## Preamble

    RBIP: 19
    Title: Rigoblock Governance
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2023-01-19

## Simple Summary

Develop the governance to formally own the Rigoblock protocol.

## Abstract

The Rigoblock governance is a set of smart contracts that formally own all Rigoblock protocol and staking smart contracts.
It is built on a flexible framework that allows any project to quickly deploy their own onchain governance.

## Motivation

One of the goals of the original whitepaper was to have the protocol owned by a decentralized governance.


## Specification

The Rigoblock governance will use active staked GRG as voting power. It will be the owner of the Rigoblock protocol smart contracts
and each action proposed to the governance will require a qualified majority (2/3) of the voting power.
It should follow a deterministic deployment to allow the same governance address on all chains where it is deployed.
It should use a deterministic storage slots approach.
It should be built on a common framework and governance strategy smart contract which includes the properties that
define a specific governance.

## Notes

The Rigoblock governance was deployed on Feb 10, 2023.

## Test Cases

https://github.com/RigoBlock/v3-contracts/tree/development/test/governance

## Implementation

https://github.com/RigoBlock/v3-contracts/blob/development/contracts/governance/RigoblockGovernance.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

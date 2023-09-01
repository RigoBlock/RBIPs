## Preamble

    RBIP: 20
    Title: Rigoblock Governance
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2023-02-08

## Simple Summary

Develop a protocol extension to allow Rigoblock smart pools to interact with the governance.

## Abstract

The Rigoblock governance is a set of smart contracts that formally own all Rigoblock protocol and staking smart contracts.
Smart pools need an extension to interact with the governance.

## Motivation

Rigoblock pools own a substantial stake in the staking system, hence votes in the governance.
In order for them to be able to participate in governance, a new adapter should be developed, deployed, and added to the protocol.


## Specification

Develop a new smart contract AGovernance which allows a smart pool to make a proposal, vote on an existing proposal and execute a successful proposal.
The adapter needs to be added to the protocol Authority smart contract. Its methods must be mapped in the same smart contract.

## Notes

The Rigoblock governance adapter was deployed on Feb 11, 2023.

## Test Cases

https://github.com/RigoBlock/v3-contracts/blob/development/test/extensions/AGovernance.spec.ts

## Implementation

https://github.com/RigoBlock/v3-contracts/blob/development/contracts/protocol/extensions/adapters/AGovernance.sol

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

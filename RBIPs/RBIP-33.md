## Preamble

    RBIP: 33
    Title: Prevent use of rogue strategy
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Governance
    Status: Draft
    Created: 2023-12-02

## Simple Summary

Explicitly verify a governance proxy's initialization parameters are correct. Will require a deployment of a new governance implementation to affect future governance proxy
deployments that use the canonical RigoBlock governance implementation. No upgrades are required on existing proxies.


## Abstract

When a governance proxy is deployed, the input parameters are validated in a strategy contract (which is also an input). The proxy deployment should revert if the strategy contract
does not implement the method assertValidInitParams, thus not validating the parameters. No action is needed on the current RigoBlock Governance proxies.

## Motivation

While this is an edge case where a user inputs a rogue strategy contract at governance deployment, the end result is that the user won't be able to re-deploy another governance proxy
with the same name after realizing the mistake. This is relevant for multi-chain governance proxies. Also, if the user inputs a rogue strategy smart contract address (which must be a
smart contract indeed), does not realize the mistake, and transfers ownership of another contract to said governance, no proposal can be made, or voted on, and the implementation cannot
be upgraded.

## Specification

To revert pool initialization without error if IRigoblockGovernanceStrategy.assertValidInitParams(params) does not execute, i.e., also in the case it fails silently (as with a smart
contract that does not implement the method). The result can be achieved without assembly or low-level calls by using a try/catch statement.

Add the following error `error StrategyParamsAssertion();` in the [MixinInitializer](https://github.com/RigoBlock/v3-contracts/blob/development/contracts/governance/mixins/MixinInitializer.sol) contract
Update params assertion as follows:
```
try IGovernanceStrategy(params.governanceStrategy).assertValidInitParams(params) {

} catch {
   revert StrategyParamsAssertion();
}
```

## Notes
It does not affect the RigoBlock Governance, but only future deployments where a rogue strategy contract is accidentally used as input.

## Test Cases
https://github.com/RigoBlock/v3-contracts/pull/398

## Implementation
https://github.com/RigoBlock/v3-contracts/pull/398


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

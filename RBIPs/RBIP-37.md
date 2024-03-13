## Preamble

    RBIP: 37
    Title: Represent GRG Staking Proxy balances as ERC4626
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Staking
    Status: Draft
    Created: 2024-03-12

## Simple Summary

Represent GRG staking proxy user balances as ERC4626.


## Abstract

Represent GRG staking proxy user balances as ERC4626 to allow easier and better data display by service providers.

## Motivation

External applications now cannot see a wallet's staked GRG balance (vault balance plus unclaimed rewards). This requires developing a custom extension to service providers like DefiLlama or Zapper.
ERC4626 provides a standard for vaults, which exposes methods that return the underlying token balance. By implementing this standard, external service providers could display staked GRG in a user's wallet and their $ value. This would be particularly useful for Etherscan display, where it is not possible to develop a custom smart contract position fetcher.

## Specification

- use OpenZeppelin ERC4626 standard;
- import interface methods;
- use their implementation;
- do not implement transfer, transferFrom and approve methods as GRG staked positions are non-transferable.

## Notes
[OpenZeppelin reference code](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/ERC4626.sol)

[ERC-4626 inflation attacks](https://blog.openzeppelin.com/a-novel-defense-against-erc4626-inflation-attacks)

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

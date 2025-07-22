## Preamble

    RBIP: 53
    Title: Mint with a token other than the base token
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
    Created: 2025-07-22

## Simple Summary

Allow minting pool tokens using a token other than the base token.


## Abstract

A new `mint` enpoint is added, which has an additional input `address selectedToken`, that allows minting smart pool tokens using
a token other than the base token. The transaction should revert if the token has not previously been declared as acceptable in the
pool storage, which requires pool operator approval.

## Motivation

Currently a mint operation is executed by transferring the base asset token from the user to the smart pool. Allowing minting with another token gives more flexibility to the users, where a different token can be used as payment. Requiring the token to being pre-
approved is a necessary step, as otherwise a user could potentially mint pool tokens with a rogue token.

## Specification

The selected token should be pre-approved as receivable in the pool by the pool operator. If not already in the active tokens list, a mint operation should prompt adding the token to the active token list. The token must have a price feed (this assertion is verified when pushing the token to the active tokens, however the balance would be null until a mint in the desired token is executed).

Having the ability to use multiple tokens for minting would allow new types of uses for the vault. For example, project/DAO treasuries could create (or delegate someone to) a pool with their own token as base token, and add both their token and i.e. a stablecoin to the pool, enabling the:

- addition of onchain liquidity to AMMs
- delegation of treasury token conversions to i.e. stablecoin (will be able to withdraw stablecoins later)
- diversification of their treasuries when they hold multiple tokens
- generation of yield on their otherwise idle assets (p2p lending, yield farming, ...)

without the need to execute a token swap beforehand.

## Test Cases
TBD.

## Implementation
TBD.


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

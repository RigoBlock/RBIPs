## Preamble

    RBIP: 1
    Title: Allow self-custody
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Core
    Status: Final
    Created: 2019-08-18

## Simple Summary

The current protocol design prevents tokens from being held outside of either the token pools smart contracts or preapproved deposit contracts. The improvement will allow, under certain conditions, for the pool operator to using self-custody solutions.

## Abstract

Meet market demand, get in parallel with industry standards, offer more heavy tested storage possibilities, open for off-chain/off-protocol trading by the pools operators.

## Motivation

There is a strong demand for cold storage solutions, which are not possible in the current implementation of the protocol. The implementation of self-custody allows for using cold-storage, meeting the traders' requirements. It allows further risk-diversification possibilities, with more complex strategies (arbitrage/hedging/market-making). It provides a bridge with legacy, highly secure service providers and is up to the standards of the industry. It also allows a quick way of implementing integrations with decentralized exchanges, given their short history and continuous breaking changes in their apis.

## Specification

- Implementation into an Drago.sol extension smart contract.
- The necessary condition is that pool operator holds a minimum GRG amount for certain thresholds
- threshold levels and amounts required based on pi number.

## Thresholds

| Transfer Amount (ETH)                                                                                                       | GRG required |
| --------------------------------------------------------------------------------------------------------------------------- | ------------ |
| [amount < 3.141592]                    |      0       |
| [3.141592 <= amount < 9.8696]                    |      98      |
| [9.8696 <= amount < 31]                    |     307      |
| [amount >= 31]                    |     962      |

## Notes

Pi number is an allegory of life. It was chosen as an arbitrary base value to make calculations, reminding the reader of how many complex solutions can be used to approximate the value of  pi, with an exponential increase in computation required in relation to precision of the estimate. It is not randomly chosen, as the self custody solution is deemed to be sufficient given the current stage of development and adoption of smart contracts technology.

## Test Cases

## Implementation

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

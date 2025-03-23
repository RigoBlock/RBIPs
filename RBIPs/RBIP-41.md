## Preamble

    RBIP: 41
    Title: Transient storage in reentrancy lock modifiers
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Final
    Created: 2024-06-06

## Simple Summary

To rewrite non-reentrant modifier using new temporary storage opcodes.


## Abstract

All reentrancy modifiers can be rewritten using the new `TSTORE` and `TLOAD` opcodes, by using temporary storage variables that get eliminated after the transaction is completed.

## Motivation

A non-reentrant modifier writes a boolean (or uint) to storage before execution and clears it after execution ends to prevent reentrancy attacks. This has a cost of 20k - 5k = 15k gas for each modifier.
The new transient storage opcodes introduced by EIP-1153 (went live on 13 March 2024) will allow writing to temporary storage at the same cost as accessing storage (i.e., 100 gas), which will lead to an ~15k gas savings per modifier, which is even more significant for nested modifiers.
At the end of the modifier execution, we must remember to clear temporary storage to prevent unexpected site effects.

## Specification
TBD. Currently the opcodes are supported in the EVM since the Dencun hardfork; in Solidity since v0.8.24 in inline assembly, which requires using/writing a library for transient modifier.
Future releases of Solidity are expected to bring support in the language, and a breaking change, i.e. v0.9, will add redesign the feature (future v0.8 language support are a temporary workaround).

Notes: [EIP-1153](https://eips.ethereum.org/EIPS/eip-1153)

## Test Cases
TBD.

## Implementation
[TReentrancyGuardTransient](https://github.com/RigoBlock/v3-contracts/blob/f291f12a8b3f07b8893038551177db030eba295c/contracts/protocol/libraries/ReentrancyGuardTransient.sol#L28)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

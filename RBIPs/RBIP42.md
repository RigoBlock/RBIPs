## Preamble

    RBIP: 42
    Title: Use try/catch statements when interacting with external applications
    Author: RB Core Team
    Type: Standard Track
    Category (*only required for Standard Track): Protocol
    Status: Draft
    Created: 2024-06-06

## Simple Summary

To use `try/catch` statements when interacting with external applications to avoid silent reverts without using assembly.


## Abstract

Using `try/catch` statements when interacting with external contracts help avoid silent reverts without using assembly and allows returning error without having to define new errors for external applications.

## Motivation

To reduce the use of inline assembly and catch errors in the underlying call. To simplify the code and reduce surface of potential vulnerabilities.


## Specification
TBD.
Notice: Sometimes we read from an external contract with staticcall to ensure we cannot execute write operations. Using a try/catch statement will not always be possible in these instances.

## Test Cases
TBD.

## Implementation
TBD.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

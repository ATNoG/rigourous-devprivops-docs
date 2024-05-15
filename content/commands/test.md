+++
title = "Test"
insert_anchor_links = "right"
+++

# Test

The test command will run every test for each test case scenario

## Parameters

- `user`: The username for authentication
- `pass`: The password for authentication
- `ip`: The triple store's IP
- `port`: The triple store's port
- `dataset`: The dataset within the triple store to use

# Tests and Test Scenarios

A test scenario consists of a system configuration, not related to the actual system, that exposes queries to pretinent test cases.
Within each test case, the specified queries are executed and the result compared to the expected result for the test case.
In the case there is a mismatch, the test fails.

The schemas used are the global ones, the regex match works the same way it does for an ordinary system analysis.

Tests require a triple store available.

The triple store is cleaned at the start of the program execution and in the end of each test scenario run, ensuring each scenario is independent from each other.


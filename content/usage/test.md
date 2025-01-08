+++
title = "Test Command"
insert_anchor_links = "right"
+++

# Test

The test command will run every test for each test scenario.

## Parameters

- `user`: The username for the triple store
- `pass`: The password for the triple store
- `ip`: The triple store's IP
- `port`: The triple store's port
- `dataset`: The dataset within the triple store to use

### Optional flags

- `--pipeline`: Whether to format the output for pipeline usage, that is, with date and time of execution and no colors.
- `--global-dir`: The path to the global directory. By default it is `/etc/devprivops`
- `--local-dir`: The path to the local directory. By default it is `.devprivops`
- `--verbose`, `-v`: Whether to display debug messages

# Tests and Test Scenarios

A test scenario consists of a system configuration, not necessarily related to the actual system, that exposes queries to pretinent test cases.
Within each test case, the specified queries are executed and the result compared to the expected result for the test case.
If the actual result differs from the expected one, the test fails.

The schemas used are the global ones and cannot be overriden, the URI regex match works the same way it does for an ordinary system analysis.

Test execution requires an accessible Fuseki triple store.
The triple store is included in the docker image, but any accessible Fuseki instance can be used.

The triple store is cleaned at the start of the program execution and in the end of each test scenario run, ensuring each scenario is independent from each other.

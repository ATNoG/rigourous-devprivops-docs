+++
title = "Analyse Command"
insert_anchor_links = "right"
+++

# Analyse

This command will run the full system analysis, produce a report, and optionally send it to a server, if its endpoint is specified. The report is generated in JSON format, to `report_<configuration name>.json`.

## Parameters

- `user`: The username for the triple store
- `pass`: The password for the triple store
- `ip`: The triple store's IP
- `port`: The triple store's port
- `dataset`: The dataset within the triple store to use. It will be cleaned before and after execution 

### Optional flags

- `--report-endpoint`: The HTTP endpoint to send the report to after the processing. If none is specified, the report is not sent anywhere
- `--pipeline`: Whether to format the output for pipeline usage, that is, with date and time of execution and no colors.
- `--global-dir`: The path to the global directory. By default it is `/etc/devprivops`
- `--local-dir`: The path to the local directory. By default it is `.devprivops`
- `--verbose`, `-v`: Whether to display debug messages

# Processing Phases

This is the rough analysis flow, for the complete flow please refer to the paper.

1. Load representation and configuration into triple store
2. Run all the reasoner rules
3. Verify policy compliance
4. Run all attack and harm trees
8. Validate whether the requirements are met
7. Check whether the violations are acceptable
9. Get extra data
0. (OPTIONAL) Send the report to the site

The triple store is cleaned before and after the execution of the command.

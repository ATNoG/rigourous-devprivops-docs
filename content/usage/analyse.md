+++
title = "Analyse"
insert_anchor_links = "right"
+++

# Analyse

This command will run the full system analysis, produce a report, and optionally send it to a server, if its endpoint is specified. The report is generated in JSON format, to `report_<configuration name>.json`.

## Parameters

- `user`: The username for authentication
- `pass`: The password for authentication
- `ip`: The triple store's IP
- `port`: The triple store's port
- `dataset`: The dataset within the triple store to use

### Optional flags

- `--report-endpoint`: The HTTP endpoint to send the report to after the processing. If none is specified, it is not sent

# Processing Phases

1. Load representation into triple store
2. Run all the reasoner rules
3. Verify policy compliance
4. Run all attack and harm trees
7. Check whether the violations are acceptable
8. Validate whether the requirements are met
9. Get extra data
0. (OPTIONAL) Send the report to the site

The triple store is cleaned before and after the execution of the command.


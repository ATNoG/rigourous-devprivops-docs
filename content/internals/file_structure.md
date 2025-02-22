+++
title = "File Structure"
insert_anchor_links = "right"
+++

# File Structure

In this page we explain how files are organized within the file system.

PrivGuide reads from 2 directories: a global one (by default `/etc/devprivops/`) and a local one (by default `.devprivops/`).
These can be changed by passing the desired directory to the options `--global-dir` and `--local-dir` respectively.

The global directory, by convention, should only have queries and description schemas that come from outside sources, like third party vendors and regulatory authorities, 
while the local directory should contain the system descriptions, company policies and schemas.
This makes it more difficult for developers to change policies and schemas from trusted third parties unnoticed, while providing ease of modification and better isolation of the parts that concern company policy.

## File lokup strategy

PrivGuide will first look for files in the local directory.
If the file cannot be found there, then it will search the global directory.
This makes it so that files on the local directory override those on the global directory.

If no global directory is specified, PrivGuide will still work, but everything it needs must be on the local directory.

## Abstract File Tree

The abstract file tree is the view the program has of both the global and local directories combined, according to the previously outlined rules.

```
devprivops/
├── attack_trees        # Where the attack and harm trees reside
│   ├── descriptions    # The tree descriptions
│   └── queries         # The queries of all trees
├── config              # All configurations of the system
├── descriptions        # All system descriptions
├── reasoner            # The reasoner rules
├── regulations         # All regulations
│   └── some_reg        # The rules of the regulation `some_reg`
│       ├── consistency # The consistency rules, as queries, of the regulation
│       ├── policies    # The policy rules, as queries, of the regulation
│       └── policies.yml# The metadata for all policies, of concistency or otherwise
├── report_data         # The arbitrary data to be included in the report
│   └── report_data.yml # The metadata of each query that fetches arbitrary data, 
│                       #     queries can be put wherever desired
├── requirements        # The requirements' descriptions and metadata
│   │                   #     queries can be put wherever desired
│   └── requirements.yml# The metadata of each requirement
├── schemas             # All the descriptions' JSON schemas 
├── tests               # All test scenarios
│   ├── scenario1       # A test scenario's description files
│   └── spec.json       # The full test and test scenario specifications
└── uris.yml            # The URI metadata for both the present system and tests
```


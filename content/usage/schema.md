+++
title = "Schema Command"
insert_anchor_links = "right"
+++

# Scehma

The `schema` command allows for the retrieval of the unchangeable schemas.
Despite not being possible to change them, it is important to be able to use them for code completion within text editors.

The unchangeable schemas are those not concerning system descriptions, that is, attack trees, queryies, extra report data and use cases.
This is the only way of accessing these schemas, as they are embeded in the binary and unchangeable.
Changes to them would render the program unusable, as, unlike descriptions, the fields of the files are hardcoded in the program and do not offer avenues for extensibility.

## Parameters

- `schema`: the schema to print, one of `attack-tree`, `query`, `report` or `requirement`


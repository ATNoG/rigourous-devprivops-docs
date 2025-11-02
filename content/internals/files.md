+++
title = "Files"
insert_anchor_links = "right"
+++

# Files

In this page, we explain which special files exist and their schemas.

## Attack/Harm Tree Descriptions

The attack tree description files contain each one attack/harm tree.
These trees are composed of nodes that are only tested if at least one of its children is possible, or it has no children.

A node follows the following structure:

```yml
- description: Node description
  query: attack_trees/queries/query_file.rq
  children: [] # list of other nodes, here depicted empty for better readability
```

## Configurations

The `config` directory contains multiple configuration files that instantiate the system's configurable variables.

This file has **no schema**, all of the values are free form and cannot be checked by schemas.
Adding schemas to this file would create a very complex multi-file schema management workflow that would get in the way of architecture design.

The file itself is an array of values

```yml
- id: prefix:variable id
  value: some value
```

The `id` field should use a prefix different from the configuration default, as using the default could create conflicts between different vendors' variables, should they have the same name.

## Regulation Policies

A policy file is an array of policy descriptions

```yml
- file: regulations/<regulation>/<consistency|policies>/rule.rq
  title: The policy title
  description: A free form text description of the policy 
  is consistency: true # Whether it is a consistency policy or not
  maximum violations: 0 # maximum number of acceptable violations
  mapping message: A message telling the developer how to deal with the results of this rule
```

Consistency policies are those that ensure the configuration does not contradict itself and that the values are within their respective domains.

## Extra Data File

The `report_data/report_data.yml` file contains a list of metadata of queries that add extra information to the report.

```yml
- url: some.id # The identifier of where the data will be inserted in the report visualizer
  query: report_data/.../query.rq # The query that will provide the data
  heading: Heading of this data point's section in the report visualizer
  description: A more lenghty description to be displayed below the heading
  data row line: ""
```

The `url` value shall be provided by the specific visualizer, as it is the responsible for organizing the information.

## Requirements

The `requirements.yml` file is a collection of use cases that contain requirements.
The requirements themselves are expressed by a query.

The use case can be a misuse case, meaning that we do not want it to be possible.

```yml
- use case: The title
  is misuse case: false # Whether it is a misuse case
  requirements: # The list of requirements
    - title: Requirement title
      description: Some more details on the requirement
      query: requirements/req.rq
```

## Test List

The list of test scenarios and tests is kept in the `tests/spec.json` file.

```json
[
    {
        "stateDir": "tests/scenario1",
        "tests": [
            {
                "query": "regulations/.../query.rq",
                "expectedResult": []
            }
        ]
    }
]
```

The file is a list of test scenarios, defined by where the scenario is stored, and a list of tests to be ran on the scenario.

Each test is then defined by the query to run and the expected result.

## URIs

The `uris.yml` file is a list of URI base metadata.

```yml
- abreviation: abrv # The short form of the URI base
  uri: https://example.com/abreviation # The long form of the URI base
  files: # List of REGEX expressions that should match the files that use it as a default base
    - .*\.abrev\.yml
```

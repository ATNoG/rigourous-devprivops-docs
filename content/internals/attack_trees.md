+++
title = "Attack and Harm Trees"
insert_anchor_links = "right"
+++

# Attack and Harm Trees

In this page, we discuss how attack and harm trees are processed.

## Representation

The syntax of the attack tree is as foolows:

```yml
- description: Node description
  query: attack_trees/queries/query_file.rq
  children: [] # list of other nodes, here depicted empty for better readability
```

## Processing

Each attack node is executed if it has no children or at least one of its children is possible.

A node can be in one of the following states:

- `NOT_EXECUTED`: When the node has not (yet) been executed
- `NOT_POSSIBLE`: When the node has been executed and its condition was determined to not occur
- `POSSIBLE`: When the node has been executed and its condition was determined to occur
- `ERROR`: When the node's execution failed


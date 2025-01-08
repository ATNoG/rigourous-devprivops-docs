+++
title = "Attack and Harm Trees"
insert_anchor_links = "right"
+++

# Attack and Harm Trees

In this page, we discuss how attack and harm trees are processed.
For more details on what attack and harm trees are, please consult the paper.

## Representation

The syntax of the attack tree is as foolows:

```yml
- description: Node description
  query: attack_trees/queries/query_file.rq
  children: [] # list of nodes, here depicted empty for better readability
```

The attack tree files should be under the `attack_trees` directory.

## Processing

Each attack node is executed if it has no children or at least one of its children is possible.
This means the leaf nodes will be the first to be executed, and then their parents according to the condition.

A node can be in one of the following states:

- `NOT_EXECUTED`: When the node has not (yet) been executed
- `NOT_POSSIBLE`: When the node has been executed and its condition was determined to not occur
- `POSSIBLE`: When the node has been executed and its condition was determined to occur
- `ERROR`: When the node's execution failed due to extraneous conditions

If the root node has state `POSSIBLE`, the attack is deemed possible and reported to the user. 

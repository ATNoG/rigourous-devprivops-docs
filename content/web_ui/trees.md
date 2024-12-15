+++
title = "Attack and Harm Trees"
insert_anchor_links = "right"
+++

# Attack/Harm Trees

These pages allow for interaction with the attack and harm trees the system must protect against.

## /trees

This endpoint shows the list of trees the system must be aware off.
It also provides means to create and delete them.

The listed trees redirect to their respective metadata pages.

## /trees/\<tree\>

This endpoint allows to edit the tree's metadata, both the original file in YAML format and a visual editor that abstracts the file editing.

It also provides a visual representation of the tree's nodes, which redirect to the nodes' queries.

## /trees/\<tree\>/\<node\>

This endpoint allows to edit the node's query file, as well as its description.

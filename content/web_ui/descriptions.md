+++
title = "System Descriptions"
insert_anchor_links = "right"
+++

# System Descriptions

These pages allow for interaction with the descriptions of the system under analysis.
They also provide visualizations through the use of plugins

## /descriptions

This endpoint shows the list of system descriptions.
It also provides means to create and delete them.

The listed descriptions redirect to their respective metadata pages.

## /descriptions/\<description\>

This endpoint allows to edit the description's original file in YAML format.

It also provides a visual representation of the description, if a WASM plugin supports this description schema.

It also provides means of changing the description's default URI, both to an existing one and a new one.

+++
title = "Visualization Plugins"
insert_anchor_links = "right"
+++

# Visualization Plugins

For a more intuitive and simple way of visualizing the system descriptions, we provide an extensible plugin system based on web assembly.

## Development

Plugins can be developed in any frammework and language that compiles to web assembly.
Javascript bindings are needed for the plugin to be integrated.

We provide an example plugin for our DFDs, written in Rust and compiled with `wasm-bindgen`.

All plugins must expose a `process_and_render` function that recieves the contents of the system description file and changes the contents of the `div` with id `mynetwork`, where the visualization will be placed. 

## Instalation

Plugins must have the name of the schema of the file type.
This means, a schema `dfd-schema.json` will serve all files `xxx.dfd.yml` and have a plugin with javascript bindings at `dfd.js`.
This file must then be put at `static/dfd/dfd.js`.

If no plugin is found, there is no visualization available.

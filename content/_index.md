+++
title = "DevPrivOps"
insert_anchor_links = "right"
+++

# PrivGuide

PrivGuide assists in system design by ensuring the system abides by privacy regulations.

This way, provided an accurate description, the system is ensured to follow privacy by design, since the architecture is itself compliant with privacy laws.

The tool can ensure regulations and requirements are met while attacks are impossible.

In the end, the tool produces a report, detailing errors.

## Ecosystem

To accomplish privacy by design, we developped several tools:

- **A CLI tool that runs the analysis**<br>
The tool reads the system description and ensures regulations and requirements are met, while also ensuring attacks are not possible. 
In the end, it condenses this information in a machine-readable report.
- **A report visualizer**<br>
The visualizer takes the machine readable format and displays it with better human readability.
It is a demonstrative visualizer, not intended for use by everyone, but to showcase possible features.
    + Impose access control based on clearence level and user group
    + Convert the report into PDF
- **A web UI interface for the tool**<br>
This web UI makes the tool more accessible to non technical users by abstracting the file edition.
    + Custom interfaces for file edition
    + Seamless colaboration and conflict resolution
    + User friendly interface with tool functions
- **This documentation website**<br>
The site serves as both documentation and advertisement for the tool

## Usage

The system is described by various **YAML files**, that are associated to **JSON schemas**, and validated through **SparQL queries**.

Users can specify their own schemas and map them to any set of description files.

The queries, schemas and descriptions can be given by any authority, company or even be provided by the user.

There is also a built-in test system for the queries, wherein a user provides a set of scenarios (test system descriptions) and specifies the expected output of the regulations to test.


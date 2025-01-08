+++
title = "Architecture"
insert_anchor_links = "right"
+++

# Architecture

ProvGuide is a simple **binary** that connects to an **Apache Jena Fuseki triple store** through HTTP, storing data processed from **YAML files**, which is reasoned upon with **SparQL queries**.

PrivGuide can be deployed locally or on a CI/CD pipeline, as depicted in the following deployment diagrams.

![Deployment in a CI/CD pipeline](/deploy_diag_pipeline.png)
![Deployment in a local machine](/deploy_diag_local.png)


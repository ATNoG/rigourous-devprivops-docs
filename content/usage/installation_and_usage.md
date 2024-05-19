+++
title = "Installation"
insert_anchor_links = "right"
+++

In this page, we discuss how to install and use the tool in some scenarios.

# Native

The tool can be isntalled nativelly by compiling the source code.
To do this, it is needed to install the dependencies in the `shell.nix`, either through `nix` itself, or by procedurally installing them through other means.

Then, to have the binary, we simply need to run `go build`.

To execute the tool, run `./devprivops <args>` or place the binary in a directory in `$PATH`.

**NOTE**: this aproach requires an instance of Apache Jena Fuseki to be set up and accessible.

# Local Docker Container

The tool can be executed from a local docker conatainer.
The container we provide already has a working Fuseki instance exposed on port 3030, without user or password, and a dataset called `tmp`.

To run the tool locally with docker containers, we can use bind mounts when starting the container

```sh
docker run -d --name devprivops -v "<host directory>:/<container directory>/:ro" devprivops
```

And then when running commands from the host, simply run

```sh
docker exec devprivops devprivops test user pass 127.0.0.1 3030 tmp --local-dir <container direcotry>
```

This will give access to the configuration directories in the host to the container.

# Pipeline

To run the tool in a pipeline, the docker container is advised.

It must reside in an accessible registry (as it is not hosted on dockerhub).

The first action to take is to start the Fuseki instance, then the commands can be executed as if the tool was natively installed.


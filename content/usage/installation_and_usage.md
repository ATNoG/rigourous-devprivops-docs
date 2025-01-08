+++
title = "Installation"
insert_anchor_links = "right"
+++

In this page, we discuss how to install and use PrivGuide in some common scenarios.

# Native

The tool can be installed nativelly by compiling the source code.
To do this, install the dependencies in `shell.nix`, either through `nix` itself (`nix-shell --extra-experimental-features "flakes"`), or by procedurally installing them through other means.

Then, to compile the binary, run `go build`.

To execute PrivGuide, run `./devprivops <args>` or place the binary in a directory in `$PATH`.

**NOTE**: this aproach requires an instance of Apache Jena Fuseki to be set up and accessible, which is not covered in this guide.

# Local Docker Container

PrivGuide can be executed from a local docker conatainer.
The provided container already has a working Fuseki instance exposed on port 3030, without user or password, and a dataset called `tmp`.
Bind mounts are used to pass the files that PrivGuide needs to analyse.

```sh
docker run -d --name privguide -v "<host directory>:/<container directory>/:ro" privguide
```

where `host directory` is where the tool files are located. By convention, it should be the local directory.

And then when running commands from the host, simply run

```sh
docker exec privguide devprivops test user pass 127.0.0.1 3030 tmp --local-dir <container direcotry>
```

This will give access to the configuration directories in the host to the container.

# Pipeline

To run the tool in a CI/CD pipeline, we can use the docker container.

It must reside in an accessible registry (as it is not hosted on dockerhub).

The first action to take is to start the Fuseki instance, then the commands can be executed as if the tool was natively installed.


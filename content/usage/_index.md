+++
title = "Commands"
insert_anchor_links = "right"
+++

# Commands

Here we describe the commands of the application.

- **analyse**: Runs the full system analysis
- **test**: Runs the tests on the queries

For more details on usage, please refer to the manpages. For inner workings please refer to the command's section on this site.

## Local Docker Containers

To run the tool locally with docker containers, we can use bind mounts when starting the container

```sh
docker run -d --name devprivops -v "<host directory>:/<container directory>/:ro" devprivops
```

And then when running commands from the host, simply run

```sh
docker exec devprivops devprivops test user pass 127.0.0.1 3030 tmp --local-dir <container direcotry>
```

This will give access to the configuration directories in the host to the container.


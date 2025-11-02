+++
title = "Usage"
insert_anchor_links = "right"
+++

# Commands

Here we describe the PrivGuide's commands.

- **analyse**: Runs the full system analysis
- **test**: Runs the tests on the queries
- **schema**: Output one of the internal schemas

For more details on usage, please refer to the respective pages. For inner workings please refer to the command's section on this site.

## Local Docker Container

To run the tool locally through its docker container, we can use bind mounts when starting the container.

```sh
docker run -d --name privguide -v "<host directory>:/<container directory>/:ro" privguide
```

PrivGuide can then be executed through `docker exec`:

```sh
docker exec privguide privguide test user pass 127.0.0.1 3030 tmp --local-dir <container directory>
```

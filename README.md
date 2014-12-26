docker-registry
===============

This repository contains resources to create a private Docker registry container which is run behind a container running Nginx.

Documentation
--------------
### Steps to create a private Docker registry:

1. [Install Docker](https://docs.docker.com/installation/#installation)
2. Clone the Dockerfiles and other config from https://github.com/ashutosh-narkar/docker-registry.git
3. **Build and Run the Docker Registry image:**
  1. cd docker
  2. docker build -rm -t registry .
  3. docker run -d --name docker_registry -p 5000:5000 -v /registry:/registry registry
     > Commands 3.i and 3.ii will create a docker image with Ubuntu 14.04 as base with Docker registry installed. This image will be used to create a daemonized Docker container called 'docker_registry'. The container will publish its port 5000 to port 5000 on the host. Finally, the data under the container's '/registry' directory will be mounted to '/registry' on the host. This directory will hold the Docker images we push to the registry.


For a tutorial on *pushing to* and *pulling from* a private Docker registry refer the page [Pull and Publish to Docker Registry](https://confluence.cyanoptics.com/display/BP2/Pull+and+Publish+to+Docker+Registry).

Creator
--------
**Ashutosh Narkar**

Copyright and license
---------------------
Copyright (C) 2014 by Cyan Inc. All rights reserved.

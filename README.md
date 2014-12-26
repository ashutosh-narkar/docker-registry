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

4. **Build and Run the Nginx Docker image:**
  1. cd nginx
  2. docker build --rm -t nginx_image .
  3. docker run -d --name nginx_container -p 8080:8080 --link docker_registry:docker_nginx nginx_image

    > Commands 4.i and 4.ii will create a docker image with Ubuntu 14.04 as base with Nginx installed. This image will be used to create a daemonized Docker container called 'nginx_container'. The container will publish its port 8080 to port 8080 on the host. The '--link' directive will enable the nginx container to obtain the IP address of the registry container which is needed to configure nginx properly.

At this point, we have a private Docker registry running behind a Nginx server which provides authentication.  Nginx access and error logs are sent to the Docker log collector on the Docker host. To view logs use *docker logs <container_name>*.
To provide SSL encryption, change the files  Dockerfile and docker-registry in the nginx directory and provide a SSL certificate.


### Tutorial on *pushing to* and *pulling from* a private Docker registry:

1. [Install Docker](https://docs.docker.com/installation/#installation)
2. **Get the SSL certificate:**
  1. wget https://github.com/ashutosh-narkar/docker-registry/blob/master/dockerregCA.crt
  2. Create a directory */usr/local/share/ca-certificates/docker-dev-cert* and copy the certificate to it
  3. Run *sudo update-ca-certificates*
  4. Repeat step2 for every machine that connects to this Docker registry
3. **Pull from Docker Registry:**
  1. docker login https://<host>:<port>
  2. docker pull <host>:<port>/<image_name>

    > If you get an error such as 'Invalid registry endpoint.....verification error', restart Docker (sudo service docker restart).

4. **Publish to Docker Registry:**:
  1. docker login https://<host>:<port>
  2. docker tag <image_name> <host>:<port>/<image_name>
  3. docker push <host>:<port>/<image_name>

*Currently only one user is authenticated to use the private Docker registry. username: superuser password: superuser . More users can be added by changing the Dockerfile in the nginx directory. To view container logs use 'docker logs <container_name>'*

Creator
--------
**Ashutosh Narkar**

Copyright and license
---------------------
Copyright (C) 2014 by Cyan Inc. All rights reserved.

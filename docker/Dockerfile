# DOCKER-VERSION  1.3.2
# AUTHOR:         Ashutosh Narkar <ashutosh.narkar@cyaninc.com>
# DESCRIPTION:    Image with docker-registry project and dependecies
# TO_BUILD:       docker build -rm -t registry .
# TO_RUN:         docker run -d --name docker_registry -p 5000:5000 -v /registry:/registry registry

# Latest Ubuntu LTS
FROM ubuntu:14.04

# Update and install development utilities
RUN apt-get update && \
    apt-get -y install build-essential python-dev libevent-dev python-pip liblzma-dev

# Install registry
RUN pip install docker-registry[bugsnag,newrelic,cors]

COPY config.yml /docker-registry/config/
ENV DOCKER_REGISTRY_CONFIG /docker-registry/config/config.yml
ENV SETTINGS_FLAVOR dev

# Expose ports
EXPOSE 5000

# Set the default command to execute
# when creating a new container
CMD ["docker-registry"]

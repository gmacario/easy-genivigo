# ===========================================================================================
# Project: https://github.com/gmacario/easy-genivigo
#
# File: goagent-docker/Dockerfile
# ===========================================================================================

FROM gocd/gocd-agent:16.2.1

MAINTAINER Gianpaolo Macario <gianpaolo_macario@mentor.com>

# Make sure the package repository is up to date
RUN apt-get update && apt-get -y dist-upgrade

# Install gosu
RUN curl -o /usr/local/bin/gosu -fsSL \
    "https://github.com/tianon/gosu/releases/download/1.7/gosu-$(dpkg --print-architecture)" && \
    chmod +x /usr/local/bin/gosu

# Install Docker
RUN curl -fsSL https://get.docker.com/ | sh
RUN usermod -aG docker go

# Workaround until user-namespaces are working
# Add user "go" to group "docker" of host (works with boot2docker)
RUN usermod -aG 100 go

# Install docker-compose
RUN curl -o /usr/local/bin/docker-compose -fsSL \
    "https://github.com/docker/compose/releases/download/1.6.2/docker-compose-$(uname -s)-$(uname -m)" && \
    chmod +x /usr/local/bin/docker-compose

# USER go
# WORKDIR /var/lib/go-agent
# CMD ["/bin/bash"]

# EOF

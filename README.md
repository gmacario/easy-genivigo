# easy-genivigo

Easily deploy a [Go.CD](https://www.go.cd/) infrastructure via [docker-machine](https://www.docker.com/docker-machine) and [docker-compose](https://www.docker.com/docker-compose).

This project may help develop and test new pipelines, agents, features, etc. locally before deploying them to <https://go.genivi.org/>.

### TL;DR

```
$ git clone https://github.com/gmacario/easy-genivigo
$ cd easy-genivigo
$ ./runme.sh
```

If the script executes successfully it will display a message like the following:

```
INFO: Browse the following URL to access the Go.CD dashboard:
INFO: http://192.168.99.100:8153
```

The Go.CD dashboard may then be accessed by opening the displayed URL using a recent Internet browser.

The behavior of the `runme.sh` script may be customized through some environment variables - please refer to the comments inside the script for details.

You will also be reminded to use the following command

```
INFO: Run the following command to configure your shell:
INFO: eval $(docker-machine env easy-genivigo)
```

in order to setup the environment variables so that `docker-compose` and `docker` will interact with the correct Docker engine.

### System Requirements

In order to run easy-genivigo you need a recent 64-bit x86 host with:

1. Minimum HW requirements: a dual-core CPU, 8 GB RAM, 100 GB disk space
2. [Docker](https://www.docker.com/) tools (see Note 1)
   * Docker Engine version 1.10.0 or later (see Note 2)
   * Docker Compose version 1.6.0 or later
   * Docker Machine version 0.6.0 or later
5. A recent Internet browser (i.e. [Google Chrome](https://www.google.com/chrome/))
6. A fast Internet connection

**Note 1**: By installing [Docker Toolbox](https://www.docker.com/products/docker-toolbox) (either on [OS X](http://www.apple.com/osx/) or [MS Windows](http://www.microsoft.com/en-us/windows)) you will get all the Docker tools (i.e. docker, docker-compose, docker-machine, etc.) required by easy-jenkins.

**Note 2**: Thanks to docker-machine you can configure easy-jenkins to deploy and run the containers on a remote Docker engine, for instance:

1. A fast, multi-core server on your local network
2. An instance on a public cloud, such as [Amazon EC2](https://aws.amazon.com/it/ec2/), [DigitalOcean](https://www.digitalocean.com/), etc.

### Architecture Overview

Here are some architecture overview of the easy-genivigo. For details please see `docker-compose.yml` and the related `Dockerfile`s.

#### Service `goserver`

This is a default Go.CD server deployed inside a Docker container

#### Service `goagent-docker`

Service `goagent-docker` instantiates a new Go.CD agent as Docker container.
This agent includes the `docker` and `docker-compose` commands which communicates with the Docker engine where both the Go.CD server and the other agents running.

To connect to other Docker engines, the agent should be customized by properly configuring the `DOCKER_xxx` environment variables.

#### Service `goagent-yocto-genivi`

Service `goagent-docker` instantiates another Go.CD agent running inside a container based on public image `gmacario/build-yocto-genivi`. See project https://github.com/gmacario/easy-build for details.

### License and Copyright

easy-genivigo is licensed under the MIT License - for details please see the `LICENSE` file.

Copyright 2016, [Gianpaolo Macario](http://gmacario.github.io/)

# easy-genivigo

Easily deploy a Go.CD infrastructure via [docker-machine](https://www.docker.com/docker-machine) and [docker-compose](https://www.docker.com/docker-compose).

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

* A modern machine with at least: dual-core CPU, 8 GB RAM, 100 GB disk space
* Docker and docker-compose (tested with [Docker Toolbox](https://www.docker.com/products/docker-toolbox) 1.10.2)
* A fast Internet connection
* A recent Internet browser

**NOTE**: Thanks to docker-machine the program also allows to deploy and run the containers on a remote Docker engine, for instance:

1. A fast, multi-core server on your local network
2. An instance on a public cloud, such as [Amazon EC2](https://aws.amazon.com/it/ec2/), [DigitalOcean](https://www.digitalocean.com/), etc.

### License

easy-genivigo is licensed under the MIT License - for details please see the `LICENSE` file.

Copyright 2016, [Gianpaolo Macario](http://gmacario.github.io/)

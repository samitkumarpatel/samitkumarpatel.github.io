---
layout: post
title: docker-machine
---

**docker machine**

[Docker Official](https://docs.docker.com/machine/) documents to be refered.

Docker Machine is a tool that lets you install Docker Engine on virtual hosts, and manage the hosts with docker-machine commands. 
You can use Machine to create Docker hosts on your local Mac or Windows box, on your company network, in your data center, or on cloud providers like Azure, AWS, or DigitalOcean

Currently docker-machine support for docker, machine, amazonec2, azure, digitalocean, google, openstack, rackspace, softlayer, virtualbox, vmwarefusion, vmwarevcloudair, vmwarevsphere, exoscale


`docker-machine --help` has:

```
Author:
  Docker Machine Contributors - <https://github.com/docker/machine>

Options:
  --debug, -D							Enable debug mode
  --storage-path, -s "/Users/samitkumarpatel/.docker/machine"	Configures storage path [$MACHINE_STORAGE_PATH]
  --tls-ca-cert 						CA to verify remotes against [$MACHINE_TLS_CA_CERT]
  --tls-ca-key 							Private key to generate certificates [$MACHINE_TLS_CA_KEY]
  --tls-client-cert 						Client cert to use for TLS [$MACHINE_TLS_CLIENT_CERT]
  --tls-client-key 						Private key used in client TLS auth [$MACHINE_TLS_CLIENT_KEY]
  --github-api-token 						Token to use for requests to the Github API [$MACHINE_GITHUB_API_TOKEN]
  --native-ssh							Use the native (Go-based) SSH implementation. [$MACHINE_NATIVE_SSH]
  --bugsnag-api-token 						BugSnag API token for crash reporting [$MACHINE_BUGSNAG_API_TOKEN]
  --help, -h							show help
  --version, -v							print the version

Commands:
  active		Print which machine is active
  config		Print the connection config for machine
  create		Create a machine
  env			Display the commands to set up the environment for the Docker client
  inspect		Inspect information about a machine
  ip			Get the IP address of a machine
  kill			Kill a machine
  ls			List machines
  provision		Re-provision existing machines
  regenerate-certs	Regenerate TLS Certificates for a machine
  restart		Restart a machine
  rm			Remove a machine
  ssh			Log into or run a command on a machine with SSH.
  scp			Copy files between machines
  mount			Mount or unmount a directory from a machine with SSHFS.
  start			Start a machine
  status		Get the status of a machine
  stop			Stop a machine
  upgrade		Upgrade a machine to the latest version of Docker
  url			Get the URL of a machine
  version		Show the Docker Machine version or a machine docker version
  help			Shows a list of commands or help for one command
```

**create**

This will create a docker-machine on virtualbox with name `node01`

```
docker-machine create --driver virtualbox node01 
```

**ls**

```
docker-machine ls
```

**env**

make all the docker-machine env variable available on your shell

```
docker-machine env master
//OR//
eval $(docker-machine env master) 
```



# Forked from servercontainers/dirvish


This repo was forked from servercontainers/dirvish and extended for the
following reasons:

## We use docker swarm.

Dirvish will often be used to remotely backup the target system. In our
docker swarm environment, where our backup resource is an NFS-mounted
filesystem and where a container will be dynamically named and assigned
to a cluster node, it is far simpler to run a local dirvish service
that will have access to the resources needed for a backup task.

## We need newer versions of database clients.

We need newer versions of the database clients for postgres and mariadb.
The versions that are available to the debian:jesse distribution are
too old to connect to the newer versions of the DBMS that are in use
today.

--------------------------------------------------------

The following text comes from the original README.md...


# Docker Dirvish Backup System (servercontainers/dirvish)
_maintained by ServerContainers_

[FAQ - All you need to know about the servercontainers Containers](https://marvin.im/docker-faq-all-you-need-to-know-about-the-marvambass-containers/)

## What is it

This Dockerfile (available as ___servercontainers/dirvish___) gives you a Dirvish Backup System in a container.

- Backup System in Container
    - _runs at a specific time in infinite loop_
- Standalone Backup
    - _container exits after backup is done_
    - good if triggered from external cronjob etc.

For Configuration of the Server you use environment Variables and volume files.

It's based on the [debian:jessie](https://registry.hub.docker.com/_/debian) Image

View in Docker Registry [servercontainers/dirvish](https://registry.hub.docker.com/u/servercontainers/dirvish/)

View in GitHub [ServerContainers/dirvish](https://github.com/ServerContainers/dirvish)

## Environment variables

- STANDALONE
    - if this variable is set to any value the container stops after the backup is done
    - default not set

- TZ
    - specify the timezone inside the container
    - default _Europe/Berlin_

- CRONTIME
    - overwrite default crontime with your one
    - default _30 4 * * *_

## Volumes

- /config
    - this is where the container looks for:
        - master.conf (dirvish main configuration file - see example or official manual)
        - id_rsa (ssh private key to connect to backup-clients/servers)
            - _don't forget to specify it inside master.conf_
        - _ida_rsa.pub (ssh public key - not needed - only on clients)_
- /backups
    - this is where the container stores backups and looks for backup-client configurations
    - _default layout beneath this folder:_ ___bank/backup/dirvish/default.conf___

_Take a look at the GitHub Repo, you'll find a example there._

## Cheat Sheet

### SSH Key generation

To connect to your hosts, you need to install a ssh keypair on them. This keypair should be moved to the config folder of the container. __Don't forget to specify it in the master.conf as well!__

To generate a keypair just execute `ssh-keygen -t rsa` and follow the instructions. You'll find your keypair beneath _~/.ssh_.

Now install the public keys on the servers you want to backup, and the private key on this container.

### Dirvish

- Initialize new vault
    - `time dirvish --init --vault name_of_vault`

---
title: Setting Up Docker SQL Database On Raspberry Pi
category: RaspberryPi
tags:
  - raspberry-pi
  - docker
  - mariadb
  - mysql
  - database
---

Estimated time: 20 minutes (+10 if older distro version).

- [Preface](#preface)
- [Installing Docker](#installing-docker)
- [Running Database Container](#running-database-container)
- [Setting Up Your Database](#setting-up-your-database)

## Preface

You can find all of the included scripts and commands in this repo:

https://github.com/kmjbyrne/docker-mysql/

Before following any guide on setting up something new, it is always advised to
update your host OS packages. This becomes more applicable if you have an older
Debian version running on an older Raspberry Pi like model 2. I wrote up this
guide using my older model 2+ which was installed in 2016 or so. A number of
issues arose with this, particularly package repos going out of date, binaries
not being found etc. I had moved house about 5 times and it was mostly sitting
in a container during this time, other life aspects took precedence. It happens.

To update OS packages:

```bash
sudo apt update
sudo apt upgrade
```

I have a couple of model 4s as well, for which the installation is much simpler
really as the scope of upgrades here was much smaller.

> Please note that this particular section only relates to your Pi if it was
> installed some time ago and is as my board was, not at all maintained.

Docker itself requires an updated version of `libseccomp` to function correctly.

Following advice from this guide: <https://docs.linuxserver.io/faq#libseccomp>

I used Option 2. However you can also just reinstall as noted in Option 3. This
would be the "best practice" approach but if you have reasons or simply don't
want to go through the ringer doing so, Option 2 will suffice (for now).

## Installing Docker

Installing Docker is a relatively simple step. Docker would not be as popular if
this wasn't the case :)

If you haven't already, update update update!

```bash
sudo apt update
sudo apt upgrade
```

Now install Docker:

```bash
sudo apt install docker
```

You will want to add docker user to own user group, for most users this will be
`pi`. Running this command will prevent the need to run `sudo docker ...` for
every docker command. This gets irritating very fast!

```bash
sudo usermod -a -G docker ${USER}
```

Docker does not automatically spin up containers after a reboot/boot so you will
want to also set this to avoid your services going offline and staying so if you
reboot (this is inevitable):

```bash
sudo systemctl enable docker
```

Docker Compose is a series of utilities that come in handy when you have
`docker-compose` configurations. If you don't need this, bypass.

To install Docker Compose:

```bash
sudo pip3 install docker-compose
```

At this point, we have completed Checkpoint #1. Hooray!

## Running Database Container

The database we will use for this exercise is MariaDB.

> MariaDB is very similar to MySQL and for most users, will have the same
> offerings. There are of course some differences and many folks believe MariaDB
> to be better of the two in fact. I have in the past, defaulted to MySQL but I
> see little difference and will likely steer towards MariaDB more and more.

For installation on a Raspberry Pi which uses ARM processor architecture MariaDB
as of `2022-01-14` is the better supported between MySQL and MariaDB.

> Fun fact: the developers of MariaDB were also of MySQL.

Docker will need to first build a database image from the Docker repo.

> The following can be broken down into a `docker-compose.yml` file but for the
> sake of keeping this lean, this will be linked as bonus material.

You will want to create a password that is not shared or stored anywhere on the
local file system in plaintext.

```bash
read -p "Please enter password for root MySQL user: " PASSWORD
```

Set a DATAPATH variable, what this does is provide the docker container with a
local path. This makes sure your database data is persistent! If you don't have
a datapath setup, if the container and image is destroyed, as will all the
database data!

```bash
DATAPATH="/var/mariadb"
```

Now, let's get that container running!

The following script is essentially telling Docker to run a new container, named
`db` on port `3306`.

`--restart unless-stopped` is signalling to Docker that this container should
remain active until manually stopped.

`-v` is an argument flag for the data volume used for persistence.

`-e` is an argument flag for environment variables which we will use to provide
the `MYSQL_ROOT_PASSWORD` to allow you to interact with the database server in
the next step.

`-d` is a flag to signal that this container should run in detached mode.
Basically running as a daemon. If this argument is not provided, Docker will use
the running terminal instance and once you terminate the session, the Docker
container will also stop.

Finally, the last positional argument is the name of the Docker image we want to
use.

For more info see: https://docs.linuxserver.io/images/docker-mariadb

```bash
docker run --name db \
    -p 3306:3306 \
    --restart unless-stopped \
    -v "${DATAPATH}:/var/lib/mysql" \
    -e MYSQL_ROOT_PASSWORD="${PASSWORD}" \
    -d linuxserver/mariadb
```

Now lets check that the container is running, if you did not see any particular
Docker errors we should expect so.

Run:

```bash
docker ps
```

You should see something similar to:

```bash
CONTAINER ID        IMAGE                 COMMAND             CREATED             STATUS              PORTS                    NAMES
[containerid]       linuxserver/mariadb   "/init"             2 hours ago         Up 2 hours          0.0.0.0:3306->3306/tcp   db
```

Where `containerid` will be equal to whatever ID Docker assigns to your MariaDB
container.

This brings us to Checkpoint #2, now we have a running database containerized
instance that is exposes port 3306 (default for MySQL).

## Setting Up Your Database

The reason this step is optional is due to the fact that some people will have
their own specific approach for this. I included this as a quick way to
bootstrap your DB and get a non-root user setup. You should not really be using
`root` user for accessing the DB from remote hosts. Security matters even for
hobbyists. It not fun to have any of your assets compromised.

<!-- ## Questions

If you have any issues with this guide, happy to answer any questions!

<contact-me /> -->

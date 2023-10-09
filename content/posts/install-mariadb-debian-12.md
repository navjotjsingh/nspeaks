---
title: "Install Mariadb Debian 12"
date: 2023-08-14T14:29:30+05:30
description: How To Install MariaDB on a Debian 12 system.
summary: How To Install MariaDB on Debian 12.
category:
    - linux
    - tutorials
tag:
    - "debian 12"
    - mariadb
---

Debian 12 supports MariaDB natively. It comes with MariaDB package which you can install directly using the following command.

```bash
sudo apt install mariadb-server
```

This will install MariaDB v10.11.3 which is fine for the most part. However, you can't choose a different version of your choice should you desire.

MariaDB has released official packages for Debian 12 but only for versions 10.11 and 11.0. To get the flexibility of installing an older version, you need to use Docker.

## Install MariaDB using Docker

The first step is to install Docker. Refer to the [official Docker documentation](https://docs.docker.com/engine/install/) for the instructions. Run the following command post installation to add your system user to the `dockers` group. This way you won't have to use `sudo` with your docker commands.

```bash
sudo usermod -aG docker ${USER}
```

Log out and log back in for the change to take affect.

Run the following command to start the MariaDB's container using the latest version.

```bash
docker run -d -v mariadb-data:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=my-secret-pw --name mariadb --restart=always mariadb:latest
```

Check the status of the running container by running `docker ps`.

```bash
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS      NAMES
9357eb83ef3f   mariadb:latest   "docker-entrypoint.s…"   6 seconds ago   Up 5 seconds   3306/tcp   mariadb
```

This creates a container called **mariadb** using the latest version. The variable `MARIADB_ROOT_PASSWORD` sets the password for the MySQL root user.

To run a different version(10.3) of MariaDB, use the following command instead.

```bash
docker run -d -v mariadb-data:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=my-secret-pw --name mariadb --restart=always mariadb:10.3
```

Connect to the MariaDB container using the following command.

```bash
docker exec -it mariadb bash
```

Log in to the MariaDB shell. You will be asked for your root password. Enter to proceed. With [MariaDB 11.0.1, Docker images don't use `mysql` symlinks](https://mariadb.com/kb/en/mariadb-11-0-1-release-notes/) so you need to use `mariadb` as the command. The `mysql` version works with lower versions.

```bash
mariadb -u root -p
```

You will get the following output.

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 11.0.2-MariaDB-1:11.0.2+maria~ubu2204 mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

So far you can access MariaDB only from the Docker network. To access it outside Docker, you need to expose its port.

Remove the existing container.

```bash
docker rm -f mariadb
```

Start the container again with the port exposed.

```bash
docker run -dp 3306:3306 -v mariadb-data:/var/lib/mysql -e MARIADB_ROOT_PASSWORD=my-secret-pw --name mariadb --restart=always mariadb:latest
```

Now you can use MariaDB with any application installed on the server as usual.

## Install MariaDB (10.11/11.0) on Debian 12

MariaDB has released official packages for Debian 12 for 10.11 and 11.0 versions. ~~They haven't updated their downloads page though.~~

Install the pre-requisites required for MariaDB.

```bash
sudo apt install apt-transport-https curl
```

Import the MariaDB signing key.

```bash
sudo curl -o /usr/share/keyrings/mariadb-keyring.pgp 'https://mariadb.org/mariadb_release_signing_key.pgp'
```

Add the MariaDB repository to the system.

```bash
sudo echo "deb [signed-by=/usr/share/keyrings/mariadb-keyring.pgp] \
https://deb.mariadb.org/10.11/debian `lsb_release -cs` main" \
  | sudo tee /etc/apt/sources.list.d/mariadb.list
```

Update the system repository list.

```bash
sudo apt update
```

Check the server status.

```bash
sudo systemctl status mariadb
```

You will get the following output.

```bash
● mariadb.service - MariaDB 10.11.5 database server
     Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; preset: enabled)
    Drop-In: /etc/systemd/system/mariadb.service.d
             └─migrated-from-my.cnf-settings.conf
     Active: active (running) since Tue 2023-08-15 09:23:34 UTC; 10s ago
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 15412 ExecStartPre=/usr/bin/install -m 755 -o mysql -g root -d /var/run/mysqld (code=exited, status=0/SUCCESS)
    Process: 15416 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
    Process: 15426 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`cd /usr/bin/..; /usr/bin/galera_recovery`; [ $? -eq 0 ]   && systemctl set-environment _WSREP_START_POSITION=$VAR>
    Process: 15475 ExecStartPost=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
    Process: 15477 ExecStartPost=/etc/mysql/debian-start (code=exited, status=0/SUCCESS)
   Main PID: 15461 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 12 (limit: 1107)
     Memory: 131.2M
        CPU: 633ms
     CGroup: /system.slice/mariadb.service
             └─15461 /usr/sbin/mariadbd
```

This concludes our tutorial on installing MariaDB on a Debian 12 machine. To find out more, go through the following resources.

- [MariaDB Documentation](https://mariadb.com/kb/en/)
- [MariaDB Docker Hub](https://hub.docker.com/_/mariadb)
- [MariaDB Downloads page](https://mariadb.org/download/?t=repo-config)
- [MariaDB Debian 12 Support Ticket](https://jira.mariadb.org/browse/MCOL-5530)

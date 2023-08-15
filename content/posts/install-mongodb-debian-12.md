---
title: "Install Mongodb on Debian 12"
date: 2023-08-08T18:51:39+05:30
description: How To Install MongoDB on a Debian 12 system.
summary: How To Install MongoDB on Debian 12 using official methods
category:
    - linux
    - tutorials
tag:
    - ubuntu
    - mongodb
---

Debian 12 (Bookworm) was released on June 10, 2023 and it has been 2 months since its release and we still don't have an official release of MongoDB for it. It's the same issue that we faced with Ubuntu 22.04 which was dependence on the old LibSSL 1.1 which was dropped with Debian 12 release.

MongoDB Team is working on adding support for Debian 12 but it will only support v7.0 for now with no plans to support the older versions. You can check the progress on MongoDB's issues page.

Till the official package is available, we will have to use workarounds to get around the limitation.

## Install MongoDB using Docker

This is probably the simplest way to get MongoDB up and running on your machine without using any workarounds. Also, even when Debian 12 is officially supported, Docker method will be the only way to install an older version(<7.0) of MongoDB.

The first step is to install Docker. Refer to the [official Docker documentation](https://docs.docker.com/engine/install/) for the instructions. Run the following command post installation to add your system user to the `dockers` group. This way you won't have to use `sudo` with your docker commands.

```bash
sudo usermod -aG docker ${USER}
```

Log out and log back in for the change to take affect.

Run the following command to start the MongoDB's docker container using the latest version.

```bash
docker run -dp 27017:27017 -v local-mongo:/data/db --name local-mongo --restart=always mongo:latest
```

Check the status of the running container by running `docker ps`.

```bash
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                           NAMES
adca52fdf919   mongo:latest   "docker-entrypoint.s…"   43 seconds ago   Up 42 seconds   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   local-mongo
```

If you want to use v4.4 of MongoDB, use the following command instead.

```bash
docker run -dp 27017:27017 -v local-mongo:/data/db --name local-mongo --restart=always mongo:4.4
```

To connect to the MongoDB container, use the following command.

```bash
docker exec -it local-mongo bash
```

To check the container logs, use the following command. The `-follow` argument allows you to watch the logs in real-time. Press **Ctrl+C** to return to the terminal.

```bash
docker logs local-mongo --follow
```

To configure your MongoDB server, pass the location to the configuration file using the `--config` flag.

```bash
docker run -d
    --name local-mongo
    -v local-data:/data/db
    -v ./mongo.conf:/etc/mongo/mongo.conf
    mongo:latest --config /etc/mongo/mongo.conf
```

You can set username and password by passing environment variables using the `-e` flag.

```bash
docker run -d
    -p 27017:27017
    --name local-mongo
    -v local-mongo:/data/db
    -e MONGODB_INITDB_ROOT_USERNAME=db-user
    -e MONGODB_INITDB_ROOT_PASSWORD=db-password
    mongo:latest
```

The above command will create a database with the provided credentials. To learn more, check the [MongoDB Docker hub](https://hub.docker.com/_/mongo) page.

## Install MongoDB using a workaround

**Note**: This method is not recommended for production environments.

If you don't want to use Docker, you can use a workaround to installing MongoDB on Debian 12. The reason MongoDB doesn't work on Debian 12 is because of the missing `libssl1.1` library. Debian 12 is using `libssl3` instead which doesn't work with the MongoDB's debian packages.

The workaround involves using the Debian 11 MongoDB package and manually installing the older version of the library.

Download the `libssl1.1` library.

```bash
wget https://security.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.1_1.1.1n-0+deb11u5_amd64.deb
```

Install the library.

```bash
sudo dpkg -i libssl1.1_1.1.1n-0+deb11u5_amd64.deb
```

Install MongoDB's GPG key.

```bash
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
   sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-6.0.gpg
```

Create the MongoDB repository file. We will use Debian 11(bullseye) release for the repository.

```bash
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] http://repo.mongodb.org/apt/debian bullseye/mongodb-org/6.0 main" | \
   sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

For 7.0 and 5.0 versions, replace 6.0 in the above commands with 7.0 and 5.0.

Install MongoDB server.

```bash
sudo apt update
sudo apt install mongodb-org
```

Enable and start MongoDB server.

```bash
sudo systemctl enable mongod --now
```

This concludes our tutorial on installing MongoDB on a Debian 12 machine. To find out more, go through the following resources.

- [MongoDB Documentation](https://www.mongodb.com/docs/)
- [MongoDB Docker Hub](https://hub.docker.com/_/mongo)
- [MongoDB Debian 12 Support Ticket](https://jira.mongodb.org/browse/SERVER-77231)
- [MongoDB Official Installation Instructions for Debian](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/)

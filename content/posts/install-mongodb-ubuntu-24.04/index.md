---
title: "Install MongoDB server in Ubuntu 24.04"
date: 2024-05-28T07:52:42+05:30
description: How to Install MongoDB in Ubuntu 24.04 system.
summary: How to Install MongoDB on Ubuntu 24.04 using official methods
category:
    - linux
    - tutorials
tag:
    - "ubuntu 24.04"
    - mongodb
---

MongoDB belongs to a family of NoSQL databases. It is a collection of JSON-like documents with no predefined schema. You can alter schema at any time and as often you like.

Ubuntu 24.04 was released on April 25, 2024 and we don't have an official release of MongoDB for it. The MongoDB team is in the process of working on a compatible version. If you can't wait, you can read on how to install it.

You can check the progress of MongoDB's official [Ubuntu 24.04 release via the official issues page](https://jira.mongodb.org/browse/SERVER-87441). I will update this article once the official release is available.

## Install MongoDB using Docker

This is probably the simplest way to get MongoDB up and running on your machine without using any hacks or workarounds. Also, you can use this method to install any version of MongoDB even if it won't be officially supported.

The first step is to install Docker. I won't cover the installation of Docker here but if you want, you can refer to the [official Docker documentation](https://docs.docker.com/engine/install/).

Run the following command to start the MongoDB's docker container using the latest version.

```bash
sudo docker run -dp 27017:27017 -v local-mongo:/data/db --name local-mongo --restart=always mongo:latest
```

To connect to the MongoDB shell, use the following command.

```bash
sudo docker exec -it local-mongo sh
```

## Install MongoDB using the official repository using a workaround

If you don't want to use Docker, there is another method. This method involves using the official MongoDB repository.

Grab the MongoDB's GPG key and add it your system. We will download the latest version of MongoDB which at the time of writing this tutorial is version 7. You should be able to install older versions as well by changing the version number in the command below.

```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```

Create the repository file for MongoDB. Since MongoDB doesn't support Ubuntu 24.04 (Noble Numbat), therefore we will use the repository for Ubuntu 22.04 (Jammy Jellyfish) for now.

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

Install MongoDB server.

```bash
sudo apt update
sudo apt install -y mongodb-org
```

## Conclusion

This concludes our tutorial on installing MongoDB on a Ubuntu 24.04 machine. To find out more, go through the following resources.

- [MongoDB Documentation](https://www.mongodb.com/docs/)
- [MongoDB Docker Hub](https://hub.docker.com/_/mongo)
- [MongoDB Ubuntu 24.04 Support Progress](https://jira.mongodb.org/browse/SERVER-87441)
- [MongoDB Official Package Install Instructions](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)

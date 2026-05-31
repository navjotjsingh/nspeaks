---
title: "Install Redis 8 in Ubuntu 26.04"
date: 2026-05-31T05:27:00Z
description: How to Install Redis 8 in Ubuntu 26.04 system.
summary: How to Install MongoDB on Ubuntu 26.04 using Redis official repository.
category:
    - linux
    - tutorials
tag:
    - "ubuntu 24.04"
    - mongodb
---

Redis is an in-memory data structure store used as a vector database, cache, document database, streaming engine, and message broker. Redis 7.2 was the last available version that was truly open-source. With Redis 7.4, it abandoned the open-source model and switched to a dual restrictive model. After severe backlash, [Redis 8 was released with a triple license](https://redis.io/legal/licenses/) with one of them being the open-source AGPLv3. Ubuntu 26.04 comes with Redis 8.0.5 which you can install using the following command.

```bash
$ sudo apt install -y redis-server
```

Redis added support for Ubuntu 26.04 with the [8.8.0 version](https://github.com/redis/redis/releases/tag/8.8.0) released on 25 May 2026.In this article, you will learn how to install Redis on Ubuntu 26.04 using the official repository and do some basic configuration to secure and improve the performance.

## Install Redis 8.0 on Ubuntu 26.04

Install the pre-requisites required for the installation.

```bash
sudo apt-get install lsb-release curl gpg
```

Import the Redis package signing key.

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/redis-archive-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/redis-archive-keyring.gpg
```

Add the Redis repository to the system.

```bash
sudo tee /etc/apt/sources.list.d/redis.sources <<EOF
Types: deb
URIs: https://packages.redis.io/deb
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: main
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/redis-archive-keyring.gpg
EOF
```

Update the system repository list.

```bash
sudo apt update
```

Install Redis server.

```bash
sudo apt install redis-server
```

Check the installed version.

```
redis-server --version
```

You should see a similar output.

```
Redis server v=8.8.0 sha=00000000:1 malloc=jemalloc-5.3.0 bits=64 build=d411285969092c36
```

Check Redis service status.

```bash
sudo systemctl status redis-server
```

You will get the following output. Ubuntu automatically starts and enables the Redis service.

```
● redis-server.service - Advanced key-value store
     Loaded: loaded (/usr/lib/systemd/system/redis-server.service; enabled; preset: enabled)
     Active: active (running) since Fri 2026-05-29 13:03:15 UTC; 17h ago
 Invocation: 246bcf9f11754b40bc111828409f2979
       Docs: http://redis.io/documentation,
             man:redis-server(1)
   Main PID: 1049 (redis-server)
     Status: "Ready to accept connections"
      Tasks: 10 (limit: 7646)
     Memory: 22.3M (peak: 55M)
        CPU: 4min 2.147s
     CGroup: /system.slice/redis-server.service
             └─1049 "/usr/bin/redis-server 127.0.0.1:6379"
May 29 13:03:15 redis systemd[1]: Starting redis-server.service - Advanced key-value store...
May 29 13:03:15 redis systemd[1]: Started redis-server.service - Advanced key-value store.
```

## Configure Redis

Redis stores its configuration in the `/etc/redis/redis.conf` file. Open the file with your choice of text editor.

```bash
sudo nano /etc/redis/redis.conf
```

By default, Redis accepts connection from `localhost`. To accept connections from outside, change the value of the `bind` variabke,

```
bind 0.0.0.0
```

To accept connections from specified hosts, you can define it in the following way. Notice the lack of commas.

```
bind 127.0.0.1 192.168.1.100
```

To prevent unauthorized access, you need to lock it using a strong password. Set the `requirepass` variable after uncommenting it in the following manner.

```
requirepass <StrongRedisPassword>
```

Choose a strong password. Redis being pretty fast, it is easy to break the password. To increase the password complexity, use the following command to generate the password.

```bash
openssl rand 60 | openssl base64 -A
```

Set a memory limit as per your system resources.

```
maxmemory 256mb
maxmemory-policy allkeys-lru
```

The `allkeys-lru` policy deletes the least recently used keys when the memory is full.

Save and close the file.

## Conclusion
This concludes our tutorial on installing Redis 8 on a Ubuntu 26.04 using the offficial Redis repository and performing some basic configuration. For more information, consult the official [Redis documentation](https://redis.io/docs/latest/).
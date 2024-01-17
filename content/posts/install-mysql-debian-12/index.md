---
title: "How To Install MySQL Server on Debian 12"
date: 2023-11-03T13:14:40+05:30
description: How to Install MySQL Server on a Debian 12 system
summary: How to Install MySQL Server on a Debian 12 using official method
category:
    - linux
    - tutorials
tag:
    - "debian 12"
    - mysql
---

Debian may have given up on MySQL but Oracle hasn't given up on Debian. MySQL Server was officially released for Debian 12 a few days back. There are several ways to install it and we will cover all of them.

## Install MySQL using APT

This is the method that I would recommend. The first step is to import the MySQL GPG key.

```bash
sudo gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/mysql-server.gpg --keyserver pgp.mit.edu --recv-keys 3A79BD29
```

This is a bit much. Let us go through all the options.

The next step is to create the MySQL sources file.

```bash
echo "deb [signed-by=/usr/share/keyrings/mysql-server.gpg] http://repo.mysql.com/apt/debian/ `lsb_release -cs` mysql-8.0" | sudo tee /etc/apt/sources.list.d/mysql-server.list
```

The above command is for MySQL 8.0.x version. If you wish to install MySQL 8.2 version, you will need to use the following command instead.

```bash
echo "deb [signed-by=/usr/share/keyrings/mysql-server.gpg] http://repo.mysql.com/apt/debian/ `lsb_release -cs` mysql-innovation" | sudo tee /etc/apt/sources.list.d/mysql-server.list
```

You can notice that I replaced `mysql-8.0` in the command with the `mysql-innovation` tag which is what the latest version is called. It is not stable so be careful if you are going to use it in a production environment.

Update the system repositories list to include the MySQL source.

```bash
sudo apt update
```

Install MySQL server.

```bash
sudo apt mysql-server
```

## Install MySQL using APT Deb

This is a much simpler way of using the APT repository. MySQL has a repository configuration tool that you can use to configure and add MySQL repository to your system.

Visit the [MySQL APT Deb page](https://dev.mysql.com/downloads/repo/apt/) and grab the URL to the latest version of the package.

Download the package to your system.

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.28-1_all.deb
```

Install the APT configuration tool.

```bash
sudo dpkg -i mysql-apt-config_0.8.28-1_all.deb
```

You will be given the following options.

![MySQL APT Configure Tool options](images/mysql-apt-tool-options.png#center)

Here you can select which packages you want to install. The default option is MySQL 8.0 along with MySQL Tools and Connectors. The Preview packages are tools which are useful if you want to test upcoming releases of MySQL. That option is disabled by default.

If you are satisfied with the default options, select the Ok option in the list and then select Ok at the bottom to proceed.

![MySQL APT Tool Default options selected](images/mysql-apt-default-options.png#center)

However, if you want to make a change for example, you want to install the latest beta version of MySQL which is v8.2, then select **MySQL Server & Cluster** option and select Ok to move to the next screen.

![MySQL version selection screen in APT Tool](images/mysql-apt-version-select.png#center)

Select the **mysql-innovation** option and click OK to proceed.

Once you are done with the tool, a MySQL source file with your chosen options will be written to the `\etc\apt\sources.list.d` folder. You can view its contents.

```bash
sudo nano /etc/apt/sources.list.d/mysql.list
```

It should look something like this

```bash
### THIS FILE IS AUTOMATICALLY CONFIGURED ###
# You may comment out entries below, but any other modifications may be lost.
# Use command 'dpkg-reconfigure mysql-apt-config' as root for modifications.
deb [signed-by=/usr/share/keyrings/mysql-apt-config.gpg] http://repo.mysql.com/apt/debian/ bookworm mysql-apt-config
deb [signed-by=/usr/share/keyrings/mysql-apt-config.gpg] http://repo.mysql.com/apt/debian/ bookworm mysql-8.0
deb [signed-by=/usr/share/keyrings/mysql-apt-config.gpg] http://repo.mysql.com/apt/debian/ bookworm mysql-tools
deb [signed-by=/usr/share/keyrings/mysql-apt-config.gpg] http://repo.mysql.com/apt/debian/ bookworm mysql-tools-preview
deb-src [signed-by=/usr/share/keyrings/mysql-apt-config.gpg] http://repo.mysql.com/apt/debian/ bookworm mysql-8.0
```

You can see I even enabled the MySQL Preview tools at my end. Yours will be different depending on the options you choose. Once you are satisfied, close the file.

The next step is to update the system repository list.

```bash
sudo apt update
```

And install the MySQL server.

```bash
sudo apt install mysql-server
```

You will be asked to choose a root password. Make sure to choose a strong one. You will be asked again to confirm it.

![MySQL Root Password screen](images/mysql-root-password.png#center)

Next, you will be asked to select the authentication method. MySQL 8 and above switched to a new authentication method called **caching_sha2_password** which is supported by a lot of web services and applications in which case stick to the default option. However, if you are unsure or using an application that is still using the older method (**mysql_native_password**), choose the **Legacy Authentication** option. Select Ok to proceed with the installation.

![MySQL Authentication Selection screen](images/mysql-auth-selection.png#center)

Once the installation is complete, MySQL service will be up and running. You can check the status using the following command.

```bash
sudo systemctl status mysql
```

## Install MySQL using Docker

It goes without saying that you need to have Docker installed. Follow the official [Docker installation instructions for Debian](https://docs.docker.com/engine/install/debian/).

The next step is to add your Linux user to the `docker` group which is created during the installation. Doing so would mean you don't need to use `sudo` to run Docker commands.

```bash
sudo usermod -aG docker $(whoami)
```

Log out and log back in for the change to go live. Or you can simply use the following command instead.

```bash
su - ${USER}
```

Next, create a directory for MySQL on your system and switch to it.

```bash
mkdir ~/mysql && cd ~/mysql
```

Create and open the `docker-compose.yml` file for editing.

```bash
nano docker-compose.yml
```

Paste the following code in it.

```yml
services:
  database:
    image: container-registry.oracle.com/mysql/community-server:latest
    container_name: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_USER: mysqluser
      MYSQL_PASSWORD: mysqlpassword
      MYSQL_DATABASE: testdb
    ports:
      - "3306:3306"
    volumes:
      - ./mysql:/var/lib/mysql
```

Save the file and close the editor by pressing **Ctrl+X** and entering **Y** when prompted. Here, we have set the root password and user credentials giving rights over a test database that will be created when the Docker container is created.

Start the MySQL container.

```bash
docker compose up -d
```

You can check the status of the container.

```bash
docker ps
CONTAINER ID   IMAGE                                                         COMMAND                  CREATED         STATUS         PORTS                                                        NAMES
ec42fb205f1e   container-registry.oracle.com/mysql/community-server:latest   "/entrypoint.sh mysq…"   4 seconds ago   Up 2 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060-33061/tcp   mysql
```

## Install MySQL using Deb packages

Did I tell you there is another method? Maybe. I guess I am just stretching the article and you are probably right. But you should know that Oracle provides binary DEB packages of MySQL as well in case you don't want to go through Docker or configuring APT.

Visit the [MySQL Downloads page](https://dev.mysql.com/downloads/) and choose the [MySQL Community Server page](https://dev.mysql.com/downloads/mysql/) and choose the version and Operating system as **Debian Linux** on the following page.

Grab the download link for the **Debian Linux 12 (x86, 64-bit), DEB Bundle** which is the topmost link.

Create a directory to store the deb packages and switch to it.

```bash
mkdir ~/mysql
cd ~/mysql
```

Download the MySQL Tar bundle into the directory.

```bash
wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-server_8.0.35-1debian12_amd64.deb-bundle.tar
```

Extract the tar file.

```bash
tar -xf mysql-server_8.0.35-1debian12_amd64.deb-bundle.tar
```

Let's see what all packages are available for installation.

```bash
ls -l
```

You will see the following output.

```bash
total 863020
-rw-r--r-- 1 navjot navjot   1689180 Oct 12 15:59 libmysqlclient-dev_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot   1359156 Oct 12 15:59 libmysqlclient21_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot     66240 Oct 12 15:59 mysql-client_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot     67456 Oct 12 15:59 mysql-common_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot   1900724 Oct 12 15:59 mysql-community-client-core_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot   1319332 Oct 12 15:59 mysql-community-client-plugins_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot   3291688 Oct 12 15:59 mysql-community-client_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot  28588640 Oct 12 16:00 mysql-community-server-core_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot  34739984 Oct 12 16:00 mysql-community-server-debug_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot     76536 Oct 12 16:01 mysql-community-server_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot  11770304 Oct 12 16:01 mysql-community-test-debug_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot 356837928 Oct 12 16:01 mysql-community-test_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot     66236 Oct 12 16:01 mysql-server_8.0.35-1debian12_amd64.deb
-rw-r--r-- 1 navjot navjot 441856000 Oct 16 08:52 mysql-server_8.0.35-1debian12_amd64.deb-bundle.tar
-rw-r--r-- 1 navjot navjot     66248 Oct 12 16:01 mysql-testsuite_8.0.35-1debian12_amd64.deb
```

These are a lot of packages. Do you need all of them? Not really. For the basic installation, you need the common file package, the client package, the client metapackage, the server package, and the server metapackage (in the same order). And you can do that using a single command.

```bash
sudo dpkg -i mysql-{common,community-client-plugins,community-client-core,community-client,client,community-server-core,community-server,server}_*.deb
```

There are also packages with server-core and client-core in the package names. These contain binaries only and are installed automatically by the standard packages. Installing them by themselves does not result in a functioning MySQL setup.

If you get a warning about unmet dependencies, use the following command to fix it.

```bash
sudo apt -f install
```

This should install MySQL server. The service should be up and running.

## Conclusion

That's it. We have learnt all the ways you can install MySQL server on a Debian 12 system. It's upto you to choose the method that suits your requirements. To find out more, go through the following resources.

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [MySQL Docker Page](https://container-registry.oracle.com/ords/ocr/ba/mysql/community-server)
- [MySQL Downloads Page](https://dev.mysql.com/downloads/)

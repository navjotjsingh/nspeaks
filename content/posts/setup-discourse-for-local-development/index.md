---
title: "Setup Discourse for Local Development"
date: "2021-03-23"
category:
  - "tutorials"
tag:
  - discourse
  - localhost
summary: Learn how to install Discourse Forum software for Local development.
slug: setup-discourse-for-local-development
---

**Last Updated: 13 July, 2024**

There are plenty of guides available for installing Discourse Forum software on Cloud and VPS hosting but if you are a developer and want to test and work on Discourse for Local development, you need a way to install it on your local PC.

This guide will show you how to install Discourse for Local Development on your PC (Windows, Linux and macOS). For Windows installation, we will be using WSL (Windows Subsystem for Linux) to install. Therefore, the commands for Linux and Windows (WSL) will remain the same unless specifically mentioned.

## Install Docker

The first step is to install Docker which is required by Discourse to work. I am going to list steps for installing Docker in Ubuntu 20.04 and the same steps can be used to install in Windows WSL, provided you are using the Ubuntu distro on it. For other distros, you can follow the [official installation guide available from Docker](https://docs.docker.com/engine/install/).

### Ubuntu 20.04 / WSL

Use the following commands to install Docker on Ubuntu 20.04.

```bash
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

The above commands will add Docker repository and install the latest stable version of Docker Engine. But you will need to use `sudo` every time to run Docker. To run Docker using the user you log-in from, use the following command.

```bash
sudo usermod -aG docker ${USER}
```

The above command will add your current user to the Docker user group allowing you to run Docker commands without invoking sudo. You will need to log out and log back in again for the command to take effect.

To check if Docker is installed and running properly, use the following command.

```bash
docker info
```

**Note for WSL:** For running the above commands under WSL, make sure you are running the commands from the local file system (`~/discourse`) and not from a path like `/mnt/c/discourse` otherwise, the commands would fail. Also, Docker won't run if you will use the `systemctl` command. The reason for that is that WSL doesn't use `systemd` but instead uses `SysV` and hence you need to change the commands. For more information regarding the issue, you can [follow the guide available from Linux Handbook](https://linuxhandbook.com/system-has-not-been-booted-with-systemd/).

If you are using Ubuntu 20.04, then the Docker process starts automatically after installation and you can start using it. The same doesn't happen in WSL since it doesn't use `systemd`. To start the process in WSL, use the following command.

```bash
sudo service docker start
```

You can check the status of the service to see if it is running.

```bash
sudo service docker status
```

### macOS

There are two ways to install Docker in macOS.

The first method is to download the dmg package from [Docker](https://docs.docker.com/desktop/install/mac-install/) and use it.

The second method is to use the following command on mac Terminal which uses [Homebrew](https://brew.sh/).

```bash
brew install docker
```

If you face any issue with macOS, you can follow this detailed guide about [setting up Docker on mac](https://medium.com/crowdbotics/a-complete-one-by-one-guide-to-install-docker-on-your-mac-os-using-homebrew-e818eb4cfc3).

## Install/Launch Discourse

Now that Docker is installed, it is time to download Discourse. Use the following command to clone the Discourse Github repository.

```bash
git clone https://github.com/discourse/discourse.git
cd discourse
```

Run the following command to initialise and install the Docker image for Discourse and set up an administrator user. The `d` directory is not an actual directory but a symlink to the actual `bin/docker` directory which is where the actual executables are.

```bash
d/boot_dev --init
```

The command will end with the following prompts about creating an administrator account. Enter your account credentials and choose `y` to make it an administrator account to finish the process.

```bash
Creating admin user...
Email:  name@domain.com
Password:
Repeat password:

Ensuring account is active!

Account created successfully with username name
Do you want to grant Admin privileges to this account? (Y/n)  y

Your account now has Admin privileges!
```

Next, run the following command to start the Rails server for Discourse.

```bash
d/rails s
```

Launch another terminal, switch to the Discourse directory and run the following command.

```bash
cd ~/discourse
d/ember-cli
```

Wait for the following lines to show in the output.

```bash
Build successful (136070ms) – Serving on http://localhost:4200/

Slowest Nodes (totalTime >= 5%)                              | Total (avg)
-------------------------------------------------------------+----------------
@embroider/webpack (1)                                       | 92088ms
Babel: admin (1)                                             | 8239ms
Babel: discourse-plugins (21)                                | 7902ms (376 ms)
DiscourseScss (2)                                            | 7262ms (3631 ms)
```

Now, launch the URL `http://localhost:4200` in your browser and you can start using the Discourse application. You need to keep both the commands running on separate terminals in order for Discourse to run. Once you are finished using, you can terminate the commands by pressing **Ctrl + C** in both the terminals.

### Plugin Testing using Symlinks

If you are testing a plugin, you don't need to copy it to the `plugins` directory. You can simply create a symbolic link (symlink) to it.

To create a symlink in Linux/WSL/macOS, use the following command.

```bash
sudo ln -s ~/your-plugin-directory ~/discourse/plugins
```

You will need to restart your Docker container for every symlink you create. To do that, use the following commands.

```bash
d/shutdown_dev
d/boot_dev
```

### Miscellaneous Commands

There are a few more commands that you might require while working with Discourse.

- To test emails, run MailHog: `d/mailhog`
- If there are missing gems, use: `d/bundle install`
- If you need to migrate the Database, use: `d/rails db:migrate RAILS_ENV=development`
- To kill the Docker container, use: `d/shutdown_dev`
- Data is persisted between invocations of the container in your source root `tmp/postgres` directory. To reset the database, use: `sudo rm -fr data`
- If you wish to globally expose the ports from the container to the network, use: `d/boot_dev -p`
- To Run Tests, use: `d/rake autospec`
- To run plugin specific tests, run `d/rake plugin:spec["your-plugin"]`

### References

This guide itself was made possible by a Discourse team member who posted it on [their forum](https://meta.discourse.org/t/beginners-guide-to-install-discourse-for-development-using-docker/102009). You can also post any bug or issue you encounter on this thread.

This is not the only way to install Docker for local development. There are other ways (which I have not tested) and may work for you. I am listing them here.

- [Beginners Guide to install Discourse on Ubuntu for Development](https://meta.discourse.org/t/beginners-guide-to-install-discourse-on-ubuntu-for-development/14727)
- [Beginners Guide to install Discourse on Windows 10 for Development](https://meta.discourse.org/t/beginners-guide-to-install-discourse-on-windows-10-for-development/75149)
- [Developer Guide for macOS](https://github.com/discourse/discourse/blob/master/docs/DEVELOPMENT-OSX-NATIVE.md)
- [Discourse Advanced Developer Guide](https://github.com/discourse/discourse/blob/master/docs/DEVELOPER-ADVANCED.md)

### Conclusion

This concludes our tutorial on how to install and use Discourse Forum Application for local development on your PC. If you have any questions or suggestions, you can reach me via [mail](mailto:navjot@nspeaks.com).

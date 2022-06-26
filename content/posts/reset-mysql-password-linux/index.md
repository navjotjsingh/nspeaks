---
title: "Reset MySQL Root Password on Ubuntu/Debian"
date: "2016-08-20"
category: 
  - "linux"
tag:
  - "mysql"
summary: Learn how to reset your MySQL Root Password on a Ubuntu or Debian system.
slug: reset-mysql-password-linux
---

Forgetting the password is a nightmare everyone has gone through. Especially if they are crucial ones like MySQL root user password. Fortunately, it's very simple to reset it back.

Just enter the following command at your terminal

```
sudo dpkg-reconfigure mysql_serverN.N
```

where N.N is MySQL version you are using. For example, if you are using MySQL 5.5, then the command becomes

```
sudo dpkg-reconfigure mysql_server5.5
```

[Source](https://help.ubuntu.com/community/MysqlPasswordReset)

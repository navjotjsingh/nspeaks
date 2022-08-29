---
title: "Install Mongodb in Ubuntu 22.04"
date: 2022-08-28T14:01:23+05:30
description: How to Install MongoDB in Ubuntu 22.04 system.
categories: linux
tags:
    - ubuntu
    - mongodb
---

MongoDB belongs to a family of NoSQL databases. It is a collection of JSON-like documents with no predefined schema. You can alter schema at any time and as often you like.

Ubuntu 22.04 was released in April 2022 and it has been 4 months since it's release and we still don't have an official release of MongoDB for the it. The issue is that MongoDB depends on LibSSL 1.1 which was dropped in the latest Ubuntu LTS release.

You can check the progress of MongoDB's official [Ubuntu 22.04 release via the official issues page](https://jira.mongodb.org/browse/SERVER-62300). I will update this article once the official release is available.

There are several workarounds to install it on Ubuntu 22.04 and we will discuss all of them.

## MongoDB 4.4 vs 5.0/6.0

Before proceeding with the installation methods for MongoDB, you need to decide whether you want to install the 4.4 or the more recent 5.0/6.0 versions. Apart from the newer features which are released with every major upgrade, there is one difference that can affect your choice.

MongoDB dropped support for the older CPUs which are still used by some hosting companies with the 5.0 version. You can read more about it in the [official MongoDB documentation](https://www.mongodb.com/docs/v6.0/administration/production-notes/#platform-support-notes).

There is a way to decide which version of MongoDB will work with your server/machine? MongoDB 5.0 and above only works with processors which support AVX/AVX2 instruction set.

Run the following commands to check if your system processor supports the said instruction set.

```shell
grep avx /proc/cpuinfo
```

To check for the AVX2 instruction set, use the following command instead.

```shell
grep avx2 /proc/cpuinfo
```

If your processor supports the instruction set, you will see a similar output.

```shell
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti ssbd ibrs ibpb stibp fsgsbase bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves md_clear flush_l1d arch_capabilities
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti ssbd ibrs ibpb stibp fsgsbase bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves md_clear flush_l1d arch_capabilities
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti ssbd ibrs ibpb stibp fsgsbase bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves md_clear flush_l1d arch_capabilities
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology cpuid pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti ssbd ibrs ibpb stibp fsgsbase bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt xsaveopt xsavec xgetbv1 xsaves md_clear flush_l1d arch_capabilities
```

If you scroll the code to the right, you will notice both `avx` and `avx2` instruction sets in the output. If get no output, it means your CPU doesn't support MongoDB 5.0 and above.

## Install MongoDB using Docker

This is probably the simplest way to get MongoDB up and running on your machine without using any hacks or workarounds.

The first step is to install Docker. I won't cover the installation of Docker here but if you want, you can refer to the [official Docker documentation](https://docs.docker.com/engine/install/).

Run the following command to start the MongoDB's docker container using the latest version.

```bash
sudo docker run -dp 27017:27017 -v local-mongo:/data/db --name local-mongo --restart=always mongo:latest
```

If you want to use v4.4 of MongoDB, use the following command instead.

```bash
sudo docker run -dp 27017:27017 -v local-mongo:/data/db --name local-mongo --restart=always mongo:4.4
```

To connect to the MongoDB shell, use the following command.

```bash
sudo docker exec -it local-mongo sh
```

## Install MongoDB using a workaround

**Note**: This method is not recommended for production environments. Hopefully, the official package should be out in a few days.

If you don't want to use Docker, there is another method. This method involves using the official MongoDB repository for an older Ubuntu release along with manual installation of the libssl1.1 package.

Download the libssl1.1 library.

```bash
wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
```

Install the package.

```bash
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.16_amd64.deb
```

Grab the MongoDB's GPG key and add it your system.

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongo.gpg
```

Create the repository file for MongoDB. For our workaround, we will use the repository for Ubuntu 20.04(focal) release.

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongo.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

For MongoDB 5.0 and 4.4 versions, replace 6.0 in the above commands with 5.0 and 4.4.

Install MongoDB server.

```bash
sudo apt update
sudo apt install mongodb-org
```

## Conclusion

This concludes our tutorial on installing MongoDB on a Ubuntu 22.04 machine. To find out more, go through the following resources.

- [MongoDB Documentation](https://www.mongodb.com/docs/)
- [MongoDB Docker Hub](https://hub.docker.com/_/mongo)
- [MongoDB Ubuntu 22.04 Support Tickets](https://jira.mongodb.org/browse/CDRIVER-4433)
- [MongoDB CPU Platform Support](https://www.mongodb.com/docs/manual/administration/production-notes/#platform-support-notes)
- [MongoDB Official Package Install Instructions](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)

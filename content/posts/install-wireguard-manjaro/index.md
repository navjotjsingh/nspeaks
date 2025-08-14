---
title: "Install Wireguard VPN on Manjaro Linux"
date: 2025-08-14T16:53:50+05:30
category:
  - "tutorials"
tag:
  - wireguard
  - manjaro
summary: How to Install Wireguard VPN on a Manjaro Linux server.
---

I am a long time Proton VPN user and recently switched to Manjaro as my secondary distro. And I realised, that Proton doesn't care much about Manjaro and neither they do. If you try to install via the unofficial [Flathub repo](https://flathub.org/apps/com.protonvpn.www), it doesn't work. They deleted the `proton-vpn-cli` AUR package and replaced it with `proton-vpn-cli-community`. It would have been fine but the issue is the package it depends on (`python-pythondialog`) has been removed as well. I am sure there are some workarounds for it but they are not worth the trouble.

I am writing this tutorial basically to make Proton VPN work but it should work for any Wireguard based VPN provided you have the config files for it.

## Install Wireguard

The first step is to install Wireguard. Fire up your terminal and paste the following command to install Wireguard.

```bash
~ sudo pacman -S wireguard-tools
```

Wireguard requires `resolvconf` to work. For that you need to install the `systemd-resolvconf` AUR package. Run the following command to follow through.

```bash
~ sudo pacman -Syu systemd-resolvconf
```

Enable and start the service.

```bash
~ sudo systemctl enable --now systemd-resolved
```

Check the status of the service.

```bash
~ sudo systemctl status systemd-resolved
```

Output:
```bash
● systemd-resolved.service - Network Name Resolution
     Loaded: loaded (/usr/lib/systemd/system/systemd-resolved.service; enabled; preset: enabled)
     Active: active (running) since Mon 2025-08-11 09:08:16 IST; 2h 33min ago
 Invocation: 090199ab142d41cebf561a08c2315f7c
       Docs: man:systemd-resolved.service(8)
             man:org.freedesktop.resolve1(5)
             https://systemd.io/WRITING_NETWORK_CONFIGURATION_MANAGERS
             https://systemd.io/WRITING_RESOLVER_CLIENTS
   Main PID: 3904 (systemd-resolve)
     Status: "Processing requests..."
      Tasks: 1 (limit: 28556)
     Memory: 5M (peak: 6.5M)
        CPU: 1.611s
     CGroup: /system.slice/systemd-resolved.service
             └─3904 /usr/lib/systemd/systemd-resolved
             
.............
```

## Generate Wireguard Configuration files

Since I am using Proton VPN, I will ofcourse use it to generate the configuration file. You can do it with any VPN service you are using. Log in to your Proton VPN account and open the **Downloads** menu from the left sidebar.

You should be at the following page.

![(Proton VPN Downloads Page)](https://i.ibb.co/Z1LqLQvQ/image.png)

Give a name to your profile, select **GNU/Linux** as the platform, and select the revelant VPN options. Once you are finished, click the **Create** button to generate the `conf` file and download it to your system.

Copy the wireguard configuration file to the `/etc/wireguard` directory.

```bash
~ sudo cp ~/Downloads/navjotSW1.conf /etc/wireguard
```

## Start Wireguard

Start the Wireguard VPN. The configuration name should match the file name without the extension. In our example, we named our configuration file as ```navjotSW1.conf```. Therefore, we will use `navjotSW1` as the profile name for the Wireguard client.

```bash
~ sudo wg-quick up navjotSW1
```

Output:

```bash
[#] ip link add dev navjotSW1 type wireguard
[#] wg setconf navjotSW1 /dev/fd/63
[#] ip -4 address add 10.2.0.2/32 dev navjotSW1
[#] ip link set mtu 1420 up dev navjotSW1
[#] resolvconf -a navjotSW1 -m 0 -x
[#] wg set navjotSW1 fwmark 51820
[#] ip -6 rule add not fwmark 51820 table 51820
[#] ip -6 rule add table main suppress_prefixlength 0
[#] ip -6 route add ::/0 dev navjotSW1 table 51820
[#] nft -f /dev/fd/63
[#] ip -4 rule add not fwmark 51820 table 51820
[#] ip -4 rule add table main suppress_prefixlength 0
[#] ip -4 route add 0.0.0.0/0 dev navjotSW1 table 51820
[#] sysctl -q net.ipv4.conf.all.src_valid_mark=1
[#] nft -f /dev/fd/63
```

To switch off the VPN, use the following command.

```bash
~ sudo wg-quick down navjotSW1
```

## Conclusion

You learned how to install and use Wireguard VPN on Manjaro Linux. It saves you the hassle of trying to locate and install the native clients which might not be available. Check the official [Wireguard website](https://www.wireguard.com/) to find out more.

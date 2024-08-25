# Raspberry Pi OS without Desktop (bookworm)

Raspberry Pi OS 64-bit with Desktop (as of August 19, 2024) is based on Debian version: 12 (bookworm). Available applications by default include:

- openssh-server, openssh-client
- build-essential, gcc, python3

`git` is not installed.

# OS

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 12 (bookworm)
Release:	12
Codename:	bookworm

$ systemctl status ssh
ssh.service - OpenBSD Secure Shell server
  Loaded: loaded (/lib/systemd/system/ssh.service; disabled; preset: enabled)
  Active: inactive

$ gcc --version
gcc (Debian 12.2.0-14) 12.2.0

$ python --version
Python 3.11.2
```

## Available disk space

```
$ df -T
Filesystem     Type     1K-blocks    Used Available Use% Mounted on
udev           devtmpfs   3728512       0   3728512   0% /dev
tmpfs          tmpfs       799748    1200    798548   1% /run
/dev/mmcblk0p2 ext4      29386388 2041616  25833116   8% /
tmpfs          tmpfs      3998736       0   3998736   0% /dev/shm
tmpfs          tmpfs         5120      16      5104   1% /run/lock
/dev/mmcblk0p1 vfat        522230   64622    457608  13% /boot/firmware
tmpfs          tmpfs       799744       0    799744   0% /run/user/1000
```

# Basic Configurations

## Setting localization

- Locale
  - en (English)
  - Philippines
  - UTF-8
- Timezone
  - Manila/Philippines
- Keyboard layout
  - Model: Logitech
  - Layout: Japanese
  - Variant: Japanese

## Static IP address

With Debian 12, `/etc/network/interfaces` file looks as follows:

```bash
$ cat /etc/network/interfaces
# interfaces(5) file used by ifup(8) and ifdown(8)
# Include files from /etc/network/interfaces.d:
source /etc/network/interfaces.d
```

If Debian 12 for Raspberry PI 4/5 is set up without defining IP network, the directory is empty and IP addresses are not defined:

```bash
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast state DOWN group default qlen 1000
    link/ether xx:xx:xx:xx:a7:1e brd ff:ff:ff:ff:ff:ff
3: wlan0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast state DOWN group default qlen 1000
```

Let's define static an IP address for `eth0`.

```bash
$ cat /etc/network/interfaces.d/eth0-static
allow-hotplug eth0
iface eth0 inet static
address 192.168.0.41
network 192.168.0.0
netmask 255.255.255.0
broadcast 192.168.0.255
gateway 192.168.0.1
dns-nameservers 192.168.0.1
```

With Linux, you can use the `ip address` command to find your PC's IP address, the broadcast address, the network address, and `netmask` in your home Internet settings:

```bash
$ ip a
1: lo: ....
2: eth0: ....
3: wlan0: ....
    inet 192.168.0.18/24 brd 192.168.0.255 scope global dynamic noprefixroute wlan0
       valid_lft 601038sec preferred_lft 601038sec
```

In the above information, you can see

- your PC's IP address: 192.168.0.18
- netmask: 24 bits (that is, 255.255.255.0)
- broadcast address: 192.168.0.255
- network address: 192.168.0.0 (IP addr ^ netmask)

With Linux, you can use the `ip route` command to find the IP gateway address available in your home Internet settings:

```bash
$ ip r
default via 192.168.0.1 dev wlan0 proto dhcp src 192.168.0.18 metric 600 
192.168.0.0/24 dev wlan0 proto kernel scope link src 192.168.0.18 metric 600 
```

With Ubuntu, you can use the `resolvectl` command to find the DNS server available in your home Internet settings:

```bash
$ resolvectl status
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (eth0)
    Current Scopes: none
         Protocols: -DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported

Link 3 (wlan0)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 192.168.0.1
       DNS Servers: 192.168.0.1
        DNS Domain: your.domain
```

## Time servers

As there are not enough servers in Philippines timezone, date and time of OS is not synchronized with the time server. This can be confirmed by executing `date` command, which would show a wrong date and time.

It is recommended to use Asia time servers in `/etc/systemd/timesyncd.conf`:

```
[Time]
NTP=0.asia.pool.ntp.org 1.asia.pool.ntp.org 2.asia.pool.ntp.org 3.asia.pool.ntp.org
```

## Make your system up-to-date

Use [`apt`](https://www.debian.org/doc/manuals/debian-faq/pkgtools.en.html) as [`aptitude`](https://www.debian.org/doc/manuals/debian-faq/uptodate.en.html) is not the recommended tool for doing upgrade from one release to another.

```bash
$ sudo apt update && sudo apt upgrade -y
```

## SSH server

As OpenSSH server is installed but configured disabled by default, use `systemctl` to enable `ssh.service` so ad to start the service upon system boot-up.

```bash
$ sudo systemctl enable ssh

$ systemctl status ssh
ssh.service - OpenBSD Secure Shell server
  Loaded: loaded (/lib/systemd/system/ssh.service; enabled; preset: enabled)
  Active: inactive (dead)
```

Enabling the service does not start it. Reboot the system and make sure `ssh` to get started on boot-up:

```bash
$ systemctl status ssh
ssh.service - OpenBSD Secure Shell server
  Loaded: loaded (/lib/systemd/system/ssh.service; enabled; preset: enabled)
  Active: active (running) since 2024-mm-dd ...

  Main PID: nnn (sshd)

mm-dd hh:mm:ss (hostname) systemd[1]: Starting ssh.service
mm-dd hh:mm:ss (hostname) sshd[nnn]: Server listening on 0.0.0 port 22
mm-dd hh:mm:ss (hostname) sshd[nnn]: Server listening on :: port 22
mm-dd hh:mm:ss (hostname) systemd[1]: Started ssh.service
```

Once `ssh.service` has started, the server is ready to accept remote access from other machines. Only first access establishes the authentication of the server:

```bash
client $ ssh 192.168.0.16
The authenticity of host '192.168.0.16 (192.168.0.16)' can't be established.
ED25519 key fingerprint is 
...
Warning: Permanently added '192.168.0.16' (ED25519) to the list of known hosts.

user@192.168.0.16's password: 
```

From onward, you can login to the server by giving the password of the user account added in the server:

```bash
user@192.168.0.16's password: 
...
Last login: Sun Aug 18 16:57:47 2024
user@rasp4b2:~ $ logout
```

Execute `logout` to end the remote session.

```bash
user@rasp4b2:~ $ logout
Connection to 192.168.0.16 closed.
client $
```

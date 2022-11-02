# Ubuntu

Ubuntu 22.10 (Kinetic Kudu) Desktop minimum installation by default includes:

- Fiirefox browser

You may want to edit `~/.bashrc` to customize the command prompt.

## OS

```bash
lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.10
Release:	22.10
Codename:	kinetic
```

## Snap Store

Snaps are applications packaged with all their dependencies to run on all popular Linux distributions from a single build. Snaps are discoverable and installable from Snap Store. Since Ubuntu 16.04 LTS (Xenial Xerus), Snap Store or `snapd` is already installed and ready to go.

The issue is that when updates for Snap Store itself are available, Snap Store is unable to install the updates because the Snap Store process is runnng. Therefore, you need to kill the process before updating Snap Store:

```bash
$ sudo killall snap-store
$ sudo snap refresh snapd
```

## Japanese Input

Ubuntu 21.04 or later installs Mozc by default, and Mozc will be activated once the Japanese Language Pack is installed. So,

- install Japanese Language Pack
- check it Mozc is activated

## C++ compilers

```bash
$ sudo apt install build-essential
$ g++ --version
```

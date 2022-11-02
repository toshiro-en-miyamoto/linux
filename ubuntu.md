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

## XSel

XSel is a command-line program for getting and setting the contents of the X selection.

```bash
$ sudo apt install xsel
```

## Git

```bash
$ sudo apt install git
```

Using the Git Credential Store helper will store your passwords unencrypted on disk, protected only by filesystem permissions.

```bash
$ git config --global user.email "your_email@example.com"
$ git config --global user.name "your name"
$ git config --global credential.helper store
```

[Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh):

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_ed25519
```

Then add the public key in your Git account:

```bash
$ xsel -b < ~./ssh/id_ed25519
```

## C++ compilers

```bash
$ sudo apt install build-essential
$ g++ --version
```



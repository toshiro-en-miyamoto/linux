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
$ xsel -b < ~/.ssh/id_ed25519.pub
```

## VS Code

VS Code is available via Snap Store.

## C++ compilers

```bash
$ sudo apt install build-essential
$ g++ --version
g++ (Ubuntu 12.2.0-3ubuntu1) 12.2.0
```

## Java

Although OpenJDK is available on the Snap Store, we will install OpenJDK with `apt` because the OpenJDK snap includes API documents and the source code of JDK.

```bash
$ sudo apt install openjdk-19-jdk

$ java --version
openjdk 19 2022-09-20
OpenJDK Runtime Environment (build 19+36-Ubuntu-2)
OpenJDK 64-Bit Server VM (build 19+36-Ubuntu-2, mixed mode, sharing)

$ javac --version
javac 19
```

Determine the directory where JDK is installed:

```bash
$ ls -l /usr/lib/jvm/
total 8
lrwxrwxrwx 1 root root   21  9月 28 23:16 java-1.19.0-openjdk-amd64 -> java-19-openjdk-amd64
drwxr-xr-x 9 root root 4096 11月  2 22:36 java-19-openjdk-amd64
drwxr-xr-x 2 root root 4096 11月  2 22:36 openjdk-19
```

Then set `JAVA_HOME` in `~/.bash_profile`:

```bash
$ tail -2 ~/.bash_profile
JAVA_HOME="/usr/lib/jvm/java-19-openjdk-amd64";
export JAVA_HOME;
```

# Raspberry Pi OS (Bullseye)

Raspberry Pi OS 64-bit with Desktop (Release date: April 4th 2022) is based on Debian version: 11 (bullseye). Its desktop, aka PIXEL, is based on LXDE. Available applications by default include:

- Chromium browser
- git
- gcc-10, perl, python3

You may want to edit `~/.bashrc` to customize the command prompt.

# OS

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 11 (bullseye)
Release:	11
Codename:	bullseye
```

## Available disk space

```
$ df -T
Filesystem     Type     1K-blocks    Used Available Use% Mounted on
/dev/root      ext4       7332696 4192272   2786536  61% /
devtmpfs       devtmpfs   3834256       0   3834256   0% /dev
tmpfs          tmpfs      3999984  161260   3838724   5% /dev/shm
tmpfs          tmpfs      1599996    1256   1598740   1% /run
tmpfs          tmpfs         5120       4      5116   1% /run/lock
/dev/mmcblk0p1 vfat        258095   31479    226617  13% /boot
tmpfs          tmpfs       799996      24    799972   1% /run/user/1000
```

## Make your system up-to-date

Use [`apt`](https://www.debian.org/doc/manuals/debian-faq/pkgtools.en.html) as [`aptitude`](https://www.debian.org/doc/manuals/debian-faq/uptodate.en.html) is not the recommended tool for doing upgrade from one release to another.

```bash
$ sudo apt update && sudo apt upgrade -y
```

## Japanese Input

Mozc on IBus is the simplest choice.

```bash
$ sudo apt install ibus-mozc
```

Upon installation, logout and login to enable Mozc. You will find the JA panel. To enable Mozc, you need to setup IBus.

- Raspberry icon > Preferences > IBus Preferences > Input Method
- In the Input Method list
  - remove Japanese - Japanese
  - add Japanese - Mozc

You might want customize Mozc such as Kana input as the default method.

# Essentials for Programming

## Git

Refer to [git credential storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage).

```bash
$ git config --global user.email "your mail.addr"
$ git config --global user.name "Your Name"
$ git config --global credential.helper store
```

Refer to [Connecting to GitHub with SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_ed25519
```

Then add the public key in your git accout.

```bash
$ xsel --clipboard < ~/.ssh/id_ed25519.pub
```

Now you can use `ssh:` to clone repositories.

## VS Code

The repository and key can also be installed manually with the following [script](https://code.visualstudio.com/docs/setup/linux):

```bash
$ wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
$ sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
$ sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
$ rm -f packages.microsoft.gpg
```

Then update the package cache and install the package using:

```bash
$ sudo apt install apt-transport-https
$ sudo apt update
$ sudo apt install code
```

# C++20

As of July 7 2022, the default C++ is version 10.2.1. According to [C++ Standards Support in GCC](https://gcc.gnu.org/projects/cxx-status.html), although GCC's support is still experimental, many C++20 features are implemented by version 10.2. Notabe exception is the Module supprt. Therefore the default C++ is enough to study C++20.

## GoogleTest and Bazel

[GoogleTest](https://google.github.io/googletest/) is a C++ testing and mocking framework. [Bazel](https://bazel.build/) is the preferred build system used by the GoogleTest team.
To [install Bazel using Bazelisk](https://bazel.build/install/bazelisk), download the [latest Bazelisk binary](https://github.com/bazelbuild/bazelisk/releases) such as `bazelisk-linux-arm64` of version 1.12.0, then

```bash
$ sudo cp ~/Downloads/bazelisk-linux-arm64 /usr/local/bin/bazel
$ rm -f ~/Downloads/bazelisk-linux-arm64
$ sudo chmod +x /usr/local/bin/bazel
$ bazel --version
2022/07/08 05:03:05 Downloading https://releases.bazel.build/5.2.0/release/bazel-5.2.0-linux-arm64...
bazel 5.2.0
$ bazel --version
bazel 5.2.0
```

For the detail on how to run GoogleTest with Bazel, visit [GoogleTest tutorial](https://google.github.io/googletest/quickstart-bazel.html).


## VS Code extensions

- *C/C++* by Microsoft: C/C++ IntelliSense, debugging, and code browsing
- *Better C++ Syntax* by Jeff Hykin: The bleeding edge of the C++ syntax

# Java 17

Install OpenJK-17 or later if not:

```bash
$ which java
$ sudo apt install openjdk-17-jdk
$ java --version
openjdk 17.0.3 2022-04-19
OpenJDK Runtime Environment (build 17.0.3+7-Debian-1deb11u1)
OpenJDK 64-Bit Server VM (build 17.0.3+7-Debian-1deb11u1, mixed mode, sharing)
```

Indentity the directory where JDK has been installed:

```bash
$ ls -l /usr/lib/jvm/
total 8
lrwxrwxrwx 1 root root   21 May  3 06:04 java-1.17.0-openjdk-arm64 -> java-17-openjdk-arm64
drwxr-xr-x 9 root root 4096 Jul  8 14:57 java-17-openjdk-arm64
drwxr-xr-x 2 root root 4096 Jul  9 12:05 openjdk-17
```

Then set `JAVA_HOME` in `/etc/environment`:

```bash
$ cat /etc/environment
JAVA_HOME="/usr/lib/jvm/java-17-openjdk-arm64"
```

## JUnit5 and Gradle

[JUnit](https://junit.org/junit5/) is a Java unit testing framework that's one of the best test methods for regression testing. Google chose [Gradle](https://gradle.org/maven-vs-gradle/) as the official build tool for Android; not because build scripts are code, but because Gradle is modeled in a way that is extensible in the most fundamental ways. To install Gradle,

```bash
$ which gradle
$ sudo apt install gradle
$ gradle --version
Gradle 4.4.1
```

## VS Code extensions

- *Extension Pack for Java* by Microsoft: a collection of popular extensions that can help write, test and debug Java applications in Visual Studio Code.
- *Gradle for Java* by Microsoft: provides a visual interface for your Gradle build

The Extension Pack for Java enables *Test task*, which allows you to run JUnit tests.

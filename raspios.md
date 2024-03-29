# Raspberry Pi OS (bookworm)

Raspberry Pi OS 64-bit with Desktop (as of March 29, 2024) is based on Debian version: 12 (bookworm). Its desktop, aka PIXEL, is based on LXDE (using GTK 2). Available applications by default include:

- Chromium browser
- git
- gcc, python3

# OS

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 12 (bookworm)
Release:	12
Codename:	bookworm

$ gcc --version
gcc (Debian 12.2.0-14) 12.2.0

$ python --version
Python 3.11.2
```

## Available disk space

```
$ df -T
Filesystem     Type     1K-blocks    Used Available Use% Mounted on
udev           devtmpfs   3946080       0   3946080   0% /dev
tmpfs          tmpfs       824176    6384    817792   1% /run
/dev/mmcblk0p2 ext4      29362632 5295580  22555372  20% /
tmpfs          tmpfs      4120800     624   4120176   1% /dev/shm
tmpfs          tmpfs         5120      48      5072   1% /run/lock
/dev/mmcblk0p1 vfat        522230   76070    446160  15% /boot/firmware
tmpfs          tmpfs       824160     144    824016   1% /run/user/1000
```

# Before connecting to the Internet

## Setting localization

- Locale
- Timesoze
- Keyboard layout
- WiFi country

## Time servers

As there are not enough servers in Philippines timezone, it is recommended to use Asia time servers in `/etc/systemd/timesyncd.conf`:

```
[Time]
NTP=0.asia.pool.ntp.org 1.asia.pool.ntp.org 2.asia.pool.ntp.org 3.asia.pool.ntp.org
```

## `bash` prompt

You may want to edit `~/.bashrc` to customize the command prompt.

```bash
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;34m\]\W \$\[\033[00m\] '
PS1='${debian_chroot:+($debian_chroot)}\W\$ '
```

# After connecting to the Internet

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
  - remove Japanese - Japanese, if exist
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

Update to the latest version:

```bash
$ sudo apt update && sudo apt upgrade -y
```

## VS Code extensions

- Code Spell Checker by Street Side Software

# C++20

## GCC

Debian bookworm desktop release comes with Build Essential, which has GCC 12.2.0 by default. According to [C++ Standards Support in GCC](https://gcc.gnu.org/projects/cxx-status.html), GCC's support is still experimental, but GCC 12.2 implements

- most C++20 language features, including module
- many [C++20 library features](https://gcc.gnu.org/onlinedocs/libstdc++/manual/status.html), but not
  - Extending chrono to Calendars and Time Zones (P0355R7, 14.1)
  - Text formatting (P0645R10, 13.1)
  - Integration of chrono with text formatting (P1361R2, 13.1)
  - Printf corner cases in `std::format` (P1652R1, 13.1)

## Clang

Debian Build Essential doesn't include Clang. Install the latest Clang, which is version 15 for Debian bookworm.

```bash
$ sudo apt update && sudo apt upgrade -y
$ sudo apt install clang-15
```

## CMake

Install the latest CMake.

## VS Code extensions

- *C/C++ Extension Pack* by Microsoft

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

# JavaScript ES6 with Node.js

[Node.js](https://nodejs.org/en/) is a JavaScript runtime built on Chrome's V8 JavaScript engine, and [npm](https://github.com/npm) stands for Node Package Manager.

```bash
$ which node
$ sudo apt install nodejs
$ node --version
V12.22.12
$ which npm
$ sudo apt install npm
$ npm --version
7.5.2
```

## Mocha

[Mocha](https://mochajs.org/) is a feature-rich JavaScript test framework running on Node.js.

```bash
$ which mocha
$ sudo apt install mocha
$ mocha --version
8.2.1
```

## VS Code extensions

VS Code supprts JavaScript execution and debug out of the box. Create `package.json` to set up a test script, then the editor displays `Debug` button.

```json
{
  "scripts": {
    "test": "mocha"
  }
}
```

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

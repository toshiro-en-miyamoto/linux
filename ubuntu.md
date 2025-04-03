# Ubuntu (24.11, oracular)

Applications installed by default include:

- Firefox
- Python 3

You may need to install the following:

- Japanese input
- git
- VS Code
- c++ compilers

## OS

```bash
~ $ lsb_release -a
Distributor ID:	Ubuntu
Description:	Ubuntu 24.10
Release:	24.10
Codename:	oracular

~ $ python3 --version
Python 3.12.7
```

### Disk Space

```bash
~ $ df -T
Filesystem     Type  1K-blocks    Used Available Use% Mounted on
tmpfs          tmpfs    812348    4264    808084   1% /run
/dev/mmcblk0p2 ext4   28608624 6543544  20797524  24% /
tmpfs          tmpfs   4061724      12   4061712   1% /dev/shm
tmpfs          tmpfs      5120      12      5108   1% /run/lock
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-journald.service
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-udev-load-credentials.service
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-tmpfiles-setup-dev-early.service
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-sysctl.service
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-sysusers.service
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-tmpfiles-setup-dev.service
/dev/mmcblk0p1 vfat     516204  191874    324331  38% /boot/firmware
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-tmpfiles-setup.service
tmpfs          tmpfs   4061724      12   4061712   1% /tmp
tmpfs          tmpfs      1024       0      1024   0% /run/credentials/systemd-resolved.service
tmpfs          tmpfs    812344     160    812184   1% /run/user/1000
```

## Basic Configurations

### `bash` Prompt

You may want to edit `~/.bashrc` to customize the command prompt:

```bash
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;34m\]\W \$\[\033[00m\] '
PS1='${debian_chroot:+($debian_chroot)}\W \$ '
```

### System Update

```bash
~ $ sudo apt update && sudo apt upgrade -y
```

### Updating Snap Store

The issue is that when updates for Snap Store itself are available, Snap Store is unable to install the updates because the Snap Store process is running. Therefore, you need to kill the process before updating Snap Store:

```bash
~ $ sudo killall snap-store
snap-store: no process found

~ $ sudo snap refresh snap-store
snap-store (2/stable) 0+git.7a3a49a6 from Canonical✓ refreshed
```

### Removing Libre Office (optional)

```bash
$ sudo apt update
$ sudo apt remove --purge libreoffice*
$ sudo apt clean
$ sudo apt-get autoremove
```

You might find that `/etc/libreoffice` directory still remains.

```bash
$ sudo rm -r /etc/libreoffice
```

### Japanese Input

Ubuntu 21.04 or later installs Mozc by default, and Mozc will be activated once the Japanese Language Pack is installed. So,

- open Language Support to install default language pack (English)
- install Japanese Language Pack on Language Support window
- restart the system to activate Mozc
- check the top bar to see if Mozc is activated
- use Mozc Setup to change preferences

### Pi-Apps (optional for Raspberry Pi)

[Pi-Apps](https://pi-apps.io/) is the most popular app store for Raspberry Pi computers. 100% open-source bash scripts (including the GUI).

Where do the apps come from? It depends! Some application binaries come directly from their official developer and pi-apps only has to provide a simple install/uninstall script (eg: Arduino, btop++, System Monitoring Center, GitHub CLI, Minecraft Bedrock, etc). Others are custom built and/or ported to ARM Linux by pi-apps developers and contributors (eg: GitHub Desktop, Minecraft Java Prism Launcher/GDLauncher, USBImager, MuseScore 4, etc).

To install Pi-Apps

```bash
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/install | bash
```

To uninstall

```bash
~/pi-apps/uninstall
```

Raspberry Pi 5 is fully supported with Ubuntu.

Pi-Apps is required to run LMMS (Let’s Make Music) as the official builds from LMMS do not include executables for ARM64 architecture.

## Essentials for Programming

### Git

Install XSel to help Git customization.

```bash
~ $ sudo apt install xsel
```

Refer to [Git Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage).

```bash
~ $ sudo apt install git

~ $ git config --global user.email "your email address"
~ $ git config --global user.name "your name"
~ $ git config --global credential.helper store
```

Refer to [Connecting to GitHub with SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh).

```bash
~ $ ssh-keygen -t ed25519 -C "your email address"
Your identification has been saved in ~/.ssh/id_ed25519
Your public key has been saved in ~/.ssh/id_ed25519.pub

~ $ eval "$(ssh-agent -s)"
Agent pid 5325

~ $ ssh-add ~/.ssh/id_ed25519
Identity added: ~/.ssh/id_ed25519 (your email address)
```

Then add the public key in your git account.

```bash
~ $ xsel --clipboard < ~/.ssh/id_ed25519.pub
```

Now you can use `ssh:` to clone repositories.

### VS Code

> `snap install code --classic` does not work, because Snap "code" is not available on stable for `arm64`.

You can install the repository and key manually with the [following](https://code.visualstudio.com/docs/setup/linux) script:

```bash
~ $ wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
~ $ sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
~ $ echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" |sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null
~ $ rm -f packages.microsoft.gpg
```

Then update the package cache and install the package:

```bash
~ $ sudo apt install apt-transport-https
~ $ sudo apt update
~ $ sudo apt install code
~ $ sudo apt update && sudo apt upgrade -y
```

VS Code customization:

- Editor: Tab size = 2

VS Code extensions:

- *Code Spell Checker* by Street Side Software

### C++

```bash
~ $ sudo apt install build-essential
~ $ g++ --version
g++ (Ubuntu 14.2.0-4ubuntu2) 14.2.0

~ $ sudo apt install cmake
~ $ cmake --version
cmake version 3.30.3
```

VS Code extensions:

- *C/C++ Extension Pack* by Microsoft, which includes
  - *C/C++*
  - *C/C++ Themes*
  - *CMake*
  - *CMake Tools*

VS Code customization:

- C/C++ > IntelliSense > C_Cpp > Default > Cpp Standard = c++23
- C/C++ > IntelliSense > C_Cpp > Default > C Standard = c23

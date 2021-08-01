# Essentials

## snapd
- `$ which snap`
- `$ sudo apt install snapd` if not installed

## Japanese Input

- install the Japanese language pack
- activate Mozc (Ubuntu 21.04 automatically activates Mozc once the Japanese language pack installation is completed)

## Git

Refer to [git credential storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage).

- `$ sudo apt install git`
- `$ git config --global user.email "your mail.addr"`
- `$ git config --global user.name "Your Name"`
- `$ git config --global credential.helper store`
- `$ git credential-store --file ~/.git-credentials store`
  - `protocol=https`
  - `host=github.com`
  - `username=`your username at github
  - `password=`the password

## C++ compilers

- `$ sudo apt install build-essential`
- `$ g++ --version`


## Firefox Developer Edition (amd64 only)

- download the package
  - access [the Mozilla site](`https://www.mozilla.org/en-US/firefox/developer/`) with your web browser to download a tar file
  - Ubuntu Archive Manager will help extract the file into `~/firefox`
  - `$ sudo mv ~/firefox /opt/firefox-developer`
- the desktop shortcut
  - `$ cd /usr/share/applications`
  - `$ sudo cp firefox.desktop firefox-developer.desktop`
  - `$ sudo chmod +x firefox-developer.desktop`
  - change items in the `firefox-developer.desktop`
    - `[Desktop Entry]`
      - `Name=Firefox Developer Edition`
      - `Exec=/opt/firefox-developer/firefox %u`
      - `Icon=/opt/firefox-developer/browser/chrome/icons/default/default64.png`
    - `[Desktop Action new-window]`
      - `Exec=/opt/firefox-developer/firefox -new-window`
    - `[Desktop Action new-private-window]`
      - `Exec=/opt/firefox-developer/firefox -private-window`
  - log out and log in to activate the changes
  - you will be able see the blue Firefox icon in the Activities (click the nine-dot icon at the end of the Favorite Apps bar)



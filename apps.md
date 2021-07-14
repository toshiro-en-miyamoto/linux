# Installing Applications

## Setting up OS

### Japanese Input
- install the Japanese language pack
- activate Mozc (Ubuntu 21.04 automatically activates Mozc once the Japanese language pack installation is completed)

### snapd
- `$ sudo apt install snapd`

## Git & Markdown

### Git
- `$ sudo apt install git`
- `$ git config --global user.email "your mail.addr"`
- `$ git config --global user.name "Your Name"`

### Editor - Ghost Writer
- `$sudo apt install ghostwriter`

### Editor - Atom (amd64 only)

- `$ sudo snap install atom --classic`
- Editor Settings > Tab Length: 2 by default
- install the platformio-ide-terminal package

## Web Browsers for Developers

### Firefox Developer Edition (amd64 only)
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

## C++ and Rust

### GCC
- GCC is a prerequisite for Rust
- `$ sudo apt install build-essential`
- `$ g++ --version`

### Rust
- `$ sudo apt install curl`, if required
- `$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
- your `.bashrc` will be modified to add `~.cargo/bin` to $PATH, so close the terminal and open another
- `$ rustc --version`
- `$ cargo --version`

## Flutter

### Prerequisites
- `$ sudo apt install curl`
- `$ sudo apt install xz-utils`
- `$ sudo apt install libglu1-mesa`

### Flutter SDK
- `$ sudo snap install flutter --classic`
- `$ flutter sdk-path`
  - downloads Dart SDK
  - builds flutter tools such as Framework and Engine
- `$ flutter doctor`
  - downloads Material fonts, Gradle Wrapper, `sky_engine`, `linux-arm64` tools, `linux-arm64/font-subset` tools
  - [✔️] Flutter (Channel master, 2.4.0-5.0.pre.47, on Ubuntu 21.04...)
  - [✖️] Chrome - develop for the web (cannot find Chrome executable...)
  - [✖️] Flutter IDE Support (No supported IDEs installed)
  - [!] Connected device -- No devices available

### Android Studio (amd64 only)

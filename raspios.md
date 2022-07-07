# Raspberry Pi OS (Bullseye)

Raspberry Pi OS 64-bit with Desktop (Release date: April 4th 2022) is based on Debian version: 11 (bullseye). Its desktop, aka PIXEL, is based on LXDE. Available applications by default include:

- Chromium browser
- git
- gcc-10, perl, python3

You may want to edit `~/.bashrc` to customize the command prompt.

## OS

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
# Debian Live USB with Persistence Configuration (Post-Install Setup)

This repository covers optional post-install setup tasks and recommended package installations for a Debian Live USB with persistence. It is designed as a follow-up to [Part 1: Debian USB with Persistence](https://github.com/padiks/debian-usb-with-persistence), which explains how to create a live USB with persistent storage.

Users can choose which steps and software to apply according to their personal preferences. This guide is intended to help streamline the setup process, improve usability, and save time when configuring your Debian Live USB environment.

---

## Contents

1. [Set Passwords](#1-set-passwords)
2. [Hold Specific Packages](#2-hold-specific-packages)
3. [Update and Upgrade System](#3-update-and-upgrade-system)
4. [Install Common Utilities (Optional)](#4-install-common-utilities-optional)
5. [Install Okular and Remove KDE Connect (Optional)](#5-install-okular-and-remove-kde-connect-optional)
6. [Install Web Server and PHP (Optional)](#6-install-web-server-and-php-optional)
7. [Install Development Tools (Optional)](#7-install-development-tools-optional)
8. [Adjust Virtual Memory (Optional)](#8-adjust-virtual-memory-optional)

---

## How to Use

1. Follow Part 1 to create your Debian Live USB with persistence.  
2. Clone or download this repository.  
3. Follow the step-by-step instructions in the Markdown files or sections below.  
4. Customize the steps as needed for your environment.

---

> **Note:** This guide is meant for Debian 12.x Live USB users. Adjust commands and package names if you are using a different version.


---

## 1. Set Passwords

```bash
sudo passwd user   # Set password for the default user
sudo passwd root   # Set root password
```

---

## 2. Hold Specific Packages

To prevent certain applications from being upgraded automatically:

```bash
sudo apt-mark hold firefox-esr-l10n-*
sudo apt-mark hold libreoffice*
sudo apt-mark hold gimp*
sudo apt-mark showhold
```

> **Note:** On a Debian 12.4 Live USB (see [Part 1 of this guide on GitHub](https://github.com/padiks/debian-usb-with-persistence)), some applications like Firefox, LibreOffice, and GIMP are pre-installed. Updating and upgrading the system may also upgrade these apps and the system to Debian 12.12, which can take a lot of time. Using `apt-mark hold` prevents these specific packages from being upgraded, saving time.

---

## 3. Update and Upgrade System

```bash
sudo apt update
sudo apt upgrade
```

Check system info:

```bash
uname -a
cat /etc/debian_version
```

---

## 4. Install Common Utilities (Optional)

Examples of utilities you might install:

```bash
sudo apt install -y htop neofetch mate-themes qpdfview qcomicbook gnome-calculator gnome-screenshot audacious vlc easytag speedtest-cli filezilla transmission bluefish falkon lynx wget zip
```

> Users can pick and choose packages based on their needs.

---

## 5. Install Okular and Remove KDE Connect (Optional)

Install Okular with extra backends:

```
sudo apt install -y okular okular-extra-backends
```

Remove KDE Connect completely:

```
sudo dpkg --purge kdeconnect
sudo apt remove -y kdeconnect
sudo apt autoremove -y
```

> **Note:** Okular provides a versatile document viewer with extra backends for better file support, including EPUB for reading novels. KDE Connect is removed in case you don’t need it on a Live USB. The commands to install Okular and remove KDE Connect are included for convenience and were adapted and tested for this Live USB setup.

---

## 6. Install Web Server and PHP (Optional)

```bash
sudo apt install -y php libapache2-mod-php php8.2-xml apache2
apt list --installed php*
```

Adjust Apache settings:

```bash
sudo nano /etc/apache2/ports.conf
# Example: Listen 8080
sudo chown -R $USER:$USER /var/www/html
ln -s /var/www/html $HOME/Public/www
chmod 755 $HOME/Public/www
chmod 644 $HOME/Public/www/*
sudo systemctl restart apache2
```

---

## 7. Install Development Tools (Optional)

```bash
sudo apt install -y nodejs git python3-dev python3-setuptools python3-pip python3-distutils python3.11-dev python3.11-venv virtualenv software-properties-common xvfb libfontconfig1 wkhtmltopdf curl
```

Install Node.js via NVM:

```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
sudo apt install -y npm
sudo npm install -g yarn live-server
```

Python adjustments (if needed):

```bash
cd /usr/lib/python3.11
sudo mv EXTERNALLY-MANAGED _EXTERNALLY-MANAGED
cd
```

> **Note:** This section is fully optional and intended for users who want a development-ready Live USB. Commands were adapted and tested for this guide, inspired by community instructions from [Shashank Shirke, Frappe Discuss](https://discuss.frappe.io/t/guide-how-to-install-erpnext-v14-on-linux-ubuntu-step-by-step-instructions/92960).

---

## 8. Adjust Virtual Memory (Optional)

On a Debian Live USB, certain memory settings can help prevent out-of-memory errors, especially when running applications that rely on large memory allocations.

Open the sysctl configuration file:

```
sudo nano /etc/sysctl.conf
```

Add or modify the following line:

```
vm.overcommit_memory = 1
```

Apply the change immediately:

```
sudo sysctl -p
```

> **Note:** This setting allows the system to allocate more memory than physically available. It can help certain applications that rely on large memory allocations run more reliably. On low-RAM systems, however, it may increase memory pressure, so consider enabling swap if needed.

> **Reference**: Adapted from [CodingTechRoom – Redis memory overcommit warning fix](https://codingtechroom.com/question/redis-memory-overcommit-warning-fix).

---

## License

This guide is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/). You are free to share and adapt the material, provided proper credit is given and adaptations are shared under the same license.

## Credits

- Written, tested, and documented by **padiks**
- Guidance & formatting help: **ChatGPT**

## Notes

- All commands in this guide require a persistent Live USB environment to retain changes.
- Package selection is **personal preference**; install only what you need.
- For any service changes (Apache, PHP, Node.js), verify paths and permissions according to your setup.
- Users are encouraged to explore additional applications based on their workflow.

## Contributing

This guide is provided as a complete post-install resource. No additional contributions are expected at this time.

---

*Enjoy configuring your Debian Live USB environment!*

---

*End of Post-Install Setup Section*

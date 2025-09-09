# Debian Live USB with Persistence Configuration (Post-Install Setup)

This repository covers optional post-install setup tasks and recommended package installations for a Debian Live USB with persistence. It is designed as a follow-up to [Part 1: Debian USB with Persistence](https://github.com/padiks/debian-usb-with-persistence), which explains how to create a live USB with persistent storage.

Users can choose which steps and software to apply according to their personal preferences. This guide is intended to help streamline the setup process, improve usability, and save time when configuring your Debian Live USB environment.

---

## Contents

1. [Set Passwords](#1-set-passwords)  
2. [Hold Specific Packages](#2-hold-specific-packages)  
3. [Install Recommended Packages](#3-install-recommended-packages)  
4. [System Tweaks and Customization](#4-system-tweaks-and-customization)  

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
sudo apt install -y htop neofetch mate-themes okular qpdfview qcomicbook gnome-calculator gnome-screenshot audacious vlc easytag speedtest-cli filezilla transmission bluefish falkon lynx wget zip
```

> Users can pick and choose packages based on their needs.

---

## 5. Install Web Server & PHP (Optional)

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
```

---

## 6. Install Development Tools (Optional)

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

> This section is fully optional and intended for users who want a development-ready Live USB.


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

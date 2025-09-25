# Debian Live USB with Persistence Configuration (Post-Install Setup)

This repository covers optional post-install setup tasks and recommended package installations for a Debian Live USB with persistence. It is designed as a follow-up to [Part 1: Debian USB with Persistence](https://github.com/padiks/debian-usb-with-persistence), which explains how to create a live USB with persistent storage.

Users can choose which steps and software to apply according to their personal preferences. This guide is intended to help streamline the setup process, improve usability, and save time when configuring your Debian Live USB environment.

---

## Contents

1. [Set Passwords](#1-set-passwords)
2. [Manage Specific Packages (Optional)](#2-manage-specific-packages-optional)
3. [Update and Upgrade System](#3-update-and-upgrade-system)
4. [Install Common Utilities (Optional)](#4-install-common-utilities-optional)
5. [Install Okular and Remove KDE Connect (Optional)](#5-install-okular-and-remove-kde-connect-optional)
6. [Install Web Server and PHP (Optional)](#6-install-web-server-and-php-optional)
7. [Install Development Tools (Optional)](#7-install-development-tools-optional)
8. [Personal Bash Tweaks (Optional)](#8-personal-bash-tweaks-optional)
9. [Disable Unnecessary Services at Boot (Optional)](#9-disable-unnecessary-services-at-boot-optional)
10. [Set Date and Time](#10-set-date-and-time)

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

## 2. Manage Specific Packages (Optional)

> **Note:** On a Debian 12.4 Live USB (see [Part 1 of this guide on GitHub](https://github.com/padiks/debian-usb-with-persistence)), some applications like Firefox, LibreOffice, and GIMP are pre-installed. Updating and upgrading the system may also upgrade these apps and the system to Debian 12.12, which can take a lot of time. Using `apt-mark hold` prevents these specific packages from being upgraded, saving time.  

You have two ways to manage these pre-installed packages:

### Option 1: Prevent Automatic Upgrades

Use `apt-mark hold` to stop specific packages from being upgraded during system updates:

```bash
sudo apt-mark hold firefox-esr-l10n-*
sudo apt-mark hold libreoffice*
sudo apt-mark hold gimp*
sudo apt-mark showhold
```

### Option 2: Remove Unused Packages

If you prefer to remove certain applications entirely (for example, if you only use Gnumeric), you can do so:

```bash
sudo apt remove --purge libreoffice*      # Keep only Gnumeric
sudo apt remove --purge gimp*
sudo apt remove firefox-esr-l10n-*
sudo apt clean
sudo apt autoremove
```

> **Tip:** Removing unused packages frees up disk space and can speed up future updates.

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
stat -c %w /
lsblk -o NAME,SIZE,MODEL,MOUNTPOINT
```

---

## 4. Install Common Utilities (Optional)

Examples of utilities you might install:

```bash
sudo apt install -y htop neofetch mate-themes qpdfview qcomicbook gnumeric gnome-calculator gnome-screenshot audacious vlc easytag speedtest-cli filezilla transmission bluefish falkon lynx wget zip
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
# Install Apache, PHP, and XML module
sudo apt install -y apache2 php libapache2-mod-php php8.2-xml

# Verify installed PHP packages
apt list --installed php*
```

### Adjust Apache settings

```bash
# Edit Apache ports configuration if you want to change the listening port
sudo nano /etc/apache2/ports.conf
# Example inside ports.conf: Listen 8080

# Set ownership of web files to your user and www-data group
sudo chown -R $USER:www-data /var/www/html

# Create a symbolic link from your web folder to ~/Public/www (optional)
ln -s /var/www/html $HOME/Public/www

# Set directory permissions to 755
sudo find /var/www/html -type d -exec chmod 755 {} \;

# Set file permissions to 644
sudo find /var/www/html -type f -exec chmod 644 {} \;

# Ensure public directory is accessible
chmod 755 $HOME/Public

# Enable Apache mod_rewrite for URL rewriting
sudo a2enmod rewrite

# Restart Apache to apply changes
sudo systemctl restart apache2
```

### ? Notes:
- $USER is dynamic and ensures commands work for any logged-in user.
- Symbolic link creation is optional; it helps access web files via ~/Public/www.
- Permissions and ownership follow common best practices for a development environment.
- Apache must be restarted for mod_rewrite changes to take effect.

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

## 8. Personal Bash Tweaks (Optional)

Customize your shell environment by editing your `.bashrc`:

```bash
# Open .bashrc in your home directory
cd
nano .bashrc
```

Add your preferred aliases:

```bash
# Quick disk info
alias d="lsblk -o NAME,SIZE,MODEL,MOUNTPOINT"

# Clear history and exit
alias h="> ~/.bash_history && history -c && exit"

# Show IP addresses
alias i="hostname -I && ip -br -c addr show"

# Reboot system
alias r="sudo reboot"

# List running services
alias s="systemctl list-units --type=service --state=running"

# Show system and Debian version
alias v="uname -a && cat /etc/debian_version"

# Zip a folder
alias t="zip -r file.zip folder"

# Shutdown system
alias z="sudo shutdown -h now"
```

Customize your prompt (PS1) with colors:

```bash
# Green current directory in prompt
PS1='\[\e[1;32m\]${PWD} \$ \[\e[0m\]'
```

> **Note:** Place these lines at the **end of your `.bashrc` file** to ensure they override previous settings.

After editing, reload `.bashrc` to apply changes:

```bash
source ~/.bashrc
```

---

## 9. Disable Unnecessary Services at Boot (Optional)

On a Debian 12.x Live USB with persistence, you may see messages or consume resources for services you don’t need, like `smartmontools`, `cups`, `avahi-daemon`, `bluetooth`, and `ModemManager`.

### Stop services immediately:

```bash
sudo systemctl stop cups avahi-daemon bluetooth ModemManager smartmontools
```

### Prevent them from starting at boot:

```bash
sudo systemctl disable cups avahi-daemon bluetooth ModemManager smartmontools
```

> **Note:** This is safe for a Live USB setup. You can re-enable any service later if needed using `sudo systemctl enable <service>`.

---

## 10. Set Date and Time

To manually set the system date and time use:

```bash
# sudo timedatectl set-time "YYYY-MM-DD HH:MM:SS"
```

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

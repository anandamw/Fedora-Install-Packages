# Server Setup Guide

This guide provides the necessary steps to set up a Fedora-based server with essential software, including Google Chrome, Visual Studio Code, PHP (multiple versions), Apache/Nginx, MariaDB, and various PHP extensions.

## 1. Install Google Chrome

To install Google Chrome on Fedora, follow these steps:

```bash
# Enable Fedora Workstation Repositories
sudo dnf install fedora-workstation-repositories

# Enable the Google Chrome repository
sudo dnf config-manager setopt google-chrome.enabled=1

# Install Google Chrome stable version
sudo dnf install google-chrome-stable

# Import the Microsoft signing key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Add the Visual Studio Code repository
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null

# Check for package updates
dnf check-update

# Install Visual Studio Code
sudo dnf install code  # For stable version
# Or for insiders version, run:
# sudo dnf install code-insiders

# Refresh the package list
sudo dnf upgrade --refresh

# Install Remi's repository for PHP versions
sudo dnf install http://rpms.remirepo.net/fedora/remi-release-41.rpm -y

# List available PHP modules
sudo dnf module list php

# Enable PHP 8.3 from Remi's repository
sudo dnf module enable php:remi-8.3 -y

# Install PHP and CLI for Apache
sudo dnf install php php-cli -y

# Install Apache (httpd)
sudo dnf install httpd

# Start Apache web server
sudo systemctl start httpd

# Enable Apache to start on boot
sudo systemctl enable httpd --now

# Check Apache status
systemctl status httpd

# Install various PHP extensions
sudo dnf install git php-cli php-fpm php-curl php-mysqlnd php-gd php-opcache php-zip php-intl php-common php-bcmath php-imagick php-xmlrpc php-json php-readline php-memcached php-redis php-mbstring php-apcu php-xml php-dom php-redis php-memcached php-memcache php-devel php-xdebug php-pcov

# Open Port 80 for HTTP
sudo firewall-cmd --permanent --add-port=80/tcp

# Open Port 443 for HTTPS
sudo firewall-cmd --permanent --add-port=443/tcp

# Apply the firewall changes
sudo firewall-cmd --reload

# Install MariaDB server
sudo dnf install mariadb-server

# Enable MariaDB to start on boot
sudo systemctl enable mariadb.service

# Start MariaDB server
sudo systemctl start mariadb.service

# Check MariaDB status
sudo systemctl status mariadb.service

# Run the MariaDB secure installation script to configure root password and secure the database
sudo mysql_secure_installation


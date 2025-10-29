![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Base Webserver Template CT🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Update system](#👉step-1-update-system)
    2. [👉Step 2: User management](#👉step-2-user-management)
    3. [👉Step 3: Switch to new user](#👉step-3-switch-to-new-user)
    4. [👉Step 4: Install SSH](#👉step-4-install-ssh)
    5. [👉Step 5: Add SSH banner](#👉step-5-add-ssh-banner)
        1. [🍳Banner text example](#🍳banner-text-example)
    6. [👉Step 6: Install Apache and PHP](#👉step-6-install-apache-and-php)
    7. [👉Step 7: Restart Apache](#👉step-7-restart-apache)
    8. [👉Step 8: Clear history](#👉step-8-clear-history)
4. [📦Extra](#📦extra)
5. [🔗Links](#🔗links)

---

## 🖖Introduction

This template provides a structured approach to setting up a basic webserver on a Linux distribution using commonly used packages such as Apache2 and PHP.

## ✨Steps

### 👉Step 1: Update system
```bash
sudo apt update && sudo apt upgrade -y
```

### 👉Step 2: User management
```bash
sudo adduser <username>
sudo usermod -aG sudo <username>

sudo do-release-upgrade -d # Optional - upgrade to latest Ubuntu LTS
```

### 👉Step 3: Switch to new user
```bash
su - <username>
```

### 👉Step 4: Install SSH
```bash
sudo apt install openssh-server
```

### 👉Step 5: Add SSH banner
```bash
sudo bash -c 'cat <<EOF > /etc/motd
█████████████████████████████████████████████████
███████████████████             █████████████████
███████████████                     █████████████
████████████                          ███████████
██████████                              █████████
████████                                 ████████
███████           ████████████             ██████
██████          ██████    ███████           █████
█████         █████           █████         █████
████         █████             █████         ████
████        █████          ████████           ███
███         █████      ████████            ██████
███         █████   ████████            █████████
████        █████████████           █████████████
████         ████████           █████████    ████
████     ███████████        ██████████      █████
███████████████████████████████████         █████
█████████████     ██████████████           ██████
█████████             █████               ███████
█████████                                ████████
████████████                           ██████████
███████████████                      ████████████
███████████████████               ███████████████
█████████████████████████████████████████████████
- Welcome to server [$(hostname)]
EOF'
```

### 👉Step 6: Install Apache and PHP
```bash
sudo apt install apache2
sudo apt install php libapache2-mod-php php-mysql
```

### 👉Step 7: Restart Apache
```bash
sudo systemctl restart apache2
```

### 👉Step 8: Clear history
```bash
history -c
```

## 📦Extra

- Enable .conf files Apache2
```bash
cd /etc/apache2/sites-available
sudo a2ensite <filename>.conf
sudo systemctl reload apache2
```

- Disable .conf files Apache2
```bash
cd /etc/apache2/sites-available
sudo a2dissite <filename>.conf
sudo systemctl reload apache2
```

- SSL Certbot Apache2
```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache
```


## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
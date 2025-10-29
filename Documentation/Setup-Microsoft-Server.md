![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Microsoft Server🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: TODO](#👉step-1-todo)
    2. [👉Step 2: Extra](#👉step-2-extra)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This tutorial explains how to install a Microsoft server on Ubuntu 24.04 LTS. This server is a Minecraft server that allows you to play Minecraft with your friends. The installation and configuration of the server is explained in this tutorial.

## ✨Steps

[Github Piston Data](https://gist.github.com/cliffano/77a982a7503669c3e1acb0a0cf6127e9?permalink_comment_id=5104349)

### 👉Step 1: TODO
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y openjdk-21-jre-headless screen imagemagick
mkdir -p ~/minecraft && cd ~/minecraft
echo '[{"uuid": "", "name": "EliasDehondt", "level": 4}]' > ops.json
wget https://piston-data.mojang.com/v1/objects/e6ec2f64e6080b9b5d9b471b291c33cc7f509733/server.jar
wget https://eliasdh.com/assets/media/images/logo-github.png -O server-icon.png
convert server-icon.png -resize 64x64\! server-icon.png
java -Xmx1G -Xms1G -jar server.jar nogui
echo "eula=true" > eula.txt
cat << EOF > server.properties
gamemode=survival
motd=Welcome to EliasDH Minecraft Server
max-players=100
difficulty=normal
pvp=true
spawn-protection=0
online-mode=true
seed=694200000097885
EOF
sudo ufw allow 25565/tcp
screen -S minecraft -d -m java -Xmx1G -Xms1G -jar server.jar nogui
```

### 👉Step 2: Extra
```bash
hostname -I

screen -r minecraft

stop

exit
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
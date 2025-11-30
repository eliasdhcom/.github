![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Docker🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Install dependencies](#👉step-1-install-dependencies)
    2. [👉Step 2: Check for missing packages](#👉step-2-check-for-missing-packages)
    3. [👉Step 3: Add Dockers official GPG key](#👉step-3-add-dockers-official-gpg-key)
    4. [👉Step 4: Add the repository to apt sources](#👉step-4-add-the-repository-to-apt-sources)
    5. [👉Step 5: Install Docker](#👉step-5-install-docker)
    6. [👉Step 6: Add user to the Docker group](#👉step-6-add-user-to-the-docker-group)
    7. [👉Step 7: Verify the installation](#👉step-7-verify-the-installation)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to installing `Docker` (Hypervisor) on the servers (nodes) in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the servers (nodes) and the supercluster.

## ✨Steps

### 👉Step 1: Install dependencies

```bash
sudo apt-get install -y apt-transport-https gnupg ca-certificates curl software-properties-common
```	


### 👉Step 2: Check for missing packages

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```	

### 👉Step 3: Add Dockers official GPG key

```bash
sudo install -m 0755 -d /etc/apt/keyrings && sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### 👉Step 4: Add the repository to apt sources

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### 👉Step 5: Install Docker

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 👉Step 6: Add user to the Docker group

```bash
sudo usermod -aG docker $USER
```

### 👉Step 7: Verify the installation

```bash
docker --version
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
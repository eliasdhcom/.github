![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup OS🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Install the necessary software packages](#👉step-1-install-the-necessary-software-packages)
    2. [👉Step 2: Set time zone](#👉step-2-set-time-zone)
    3. [👉Step 3: Set the hostname of the servers](#👉step-3-set-the-hostname-of-the-servers)
    4. [👉Step 4: Set logging welcome message](#👉step-4-set-logging-welcome-message)
    5. [👉Step 5: Disable cloud-init on the servers](#👉step-5-disable-cloud-init-on-the-servers)
    6. [👉Step 6: Configure the network interfaces of the servers](#👉step-6-configure-the-network-interfaces-of-the-servers)
    7. [👉Step 7: Disable IPv6 on the servers](#👉step-7-disable-ipv6-on-the-servers)
    8. [👉Step 8: Add the hostnames to the hosts file](#👉step-8-add-the-hostnames-to-the-hosts-file)
    9. [👉Step 9: Apply the network configuration](#👉step-9-apply-the-network-configuration)
    10. [👉Step 10: Reboot the servers](#👉step-10-reboot-the-servers)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to setting up the `operating system` on the servers (nodes) in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the servers (nodes) and the supercluster.

## ✨Steps

### 👉Step 1: Install the necessary software packages

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
```

### 👉Step 2: Set time zone

```bash
sudo timedatectl set-timezone Europe/Brussels
```

### 👉Step 3: Set the hostname of the servers

```bash
sudo hostnamectl set-hostname node00 # e.g. node01
```

### 👉Step 4: Set logging welcome message

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

### 👉Step 5: Disable cloud-init on the servers

```bash
echo "network: {config: disabled}" | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg && sudo touch /etc/cloud/cloud-init.disabled
```

### 👉Step 6: Configure the network interfaces of the servers

```bash
sudo rm -rf /etc/netplan/50-cloud-init.yaml && sudo bash -c 'cat <<EOF > /etc/netplan/01-netcfg.yaml
network:
  version: 2
  ethernets:
    ens18:
      dhcp4: no
      dhcp6: no
      accept-ra: no
      addresses:
        - $1/24
      nameservers:
        addresses:
          - $2
          - 8.8.8.8
          - 1.1.1.1
      routes:
        - to: default
          via: $3
EOF
sudo netplan apply' && sudo bash script.sh 10.1.0.1 10.1.0.253 10.1.0.254 # Static IP, DNS IP, Gateway IP
```

### 👉Step 7: Disable IPv6 on the servers

```bash
echo -e "net.ipv6.conf.all.disable_ipv6 = 1\nnet.ipv6.conf.default.disable_ipv6 = 1\nnet.ipv6.conf.lo.disable_ipv6 = 1" | sudo tee /etc/sysctl.conf | sudo sysctl -p
```

### 👉Step 8: Add the hostnames to the hosts file

```bash
sudo bash -c 'echo -e "127.0.0.1\tlocalhost\n$(seq -f "10.1.0.%g\tnode%02g" 1 99)" > /etc/hosts'
sudo bash -c 'echo -e "127.0.0.1\tlocalhost\n$(seq -f "10.2.0.%g\tnode%02g" 1 99)" > /etc/hosts'
sudo bash -c 'echo -e "127.0.0.1\tlocalhost\n$(seq -f "10.3.0.%g\tnode%02g" 1 99)" > /etc/hosts'


echo -e "10.1.0.0 proxy01" | sudo tee /etc/hosts > /dev/null
echo -e "10.2.0.0 proxy01" | sudo tee /etc/hosts > /dev/null
echo -e "10.3.0.0 proxy01" | sudo tee /etc/hosts > /dev/null
```

### 👉Step 9: Apply the network configuration

```bash
sudo chmod 600 /etc/netplan/01-netcfg.yaml
sudo chown root:root /etc/netplan/01-netcfg.yaml
sudo netplan apply
```

### 👉Step 10: Reboot the servers

```bash
sudo reboot
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com

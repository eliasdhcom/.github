![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Kubernetes🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Disable swap on the node](#👉step-1-disable-swap-on-the-node)
    2. [👉Step 2: Load the necessary kernel modules](#👉step-2-load-the-necessary-kernel-modules)
    3. [👉Step 3: Enable IP forwarding on the node](#👉step-3-enable-ip-forwarding-on-the-node)
    4. [👉Step 4: Configure the container runtime](#👉step-4-configure-the-container-runtime)
    5. [👉Step 5: Install Kubernetes](#👉step-5-install-kubernetes)
    6. [👉Step 6: Install Kubernetes tools](#👉step-6-install-kubernetes-tools)
    7. [👉Step 7 Reboot the servers](#👉step-7-reboot-the-servers)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to installing `Kubernetes` on the servers (nodes) in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the servers (nodes) and the supercluster.

## ✨Steps

### 👉Step 1: Disable swap on the node

```bash
sudo swapoff -a && sudo sed -i 's|/swap.img|#/swap.img|g' /etc/fstab
```

### 👉Step 2: Load the necessary kernel modules

```bash
sudo modprobe overlay && sudo modprobe br_netfilter && echo -e "overlay\nbr_netfilter" | sudo tee /etc/modules-load.d/k8s.conf
```

### 👉Step 3: Enable IP forwarding on the node

```bash
echo -e "net.ipv4.ip_forward = 1" | sudo tee /etc/sysctl.d/k8s.conf && sudo sysctl --system
```

### 👉Step 4: Configure the container runtime

```bash
sudo containerd config default | sudo tee /etc/containerd/config.toml > /dev/null
sudo sed -i 's|SystemdCgroup = false|SystemdCgroup = true|g' /etc/containerd/config.toml
sudo systemctl restart containerd
```

### 👉Step 5: Install Kubernetes

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
```

### 👉Step 6: Install Kubernetes tools

```bash
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

### 👉Step 7 Reboot the servers

```bash
sudo reboot
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
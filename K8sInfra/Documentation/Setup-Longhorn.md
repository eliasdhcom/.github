![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Longhorn🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Test if the system is ready for Longhorn](#👉step-1-test-if-the-system-is-ready-for-longhorn)
    2. [👉Step 2: Install Longhorn](#👉step-2-install-longhorn)
    3. [👉Step 3: Check the status of the pods](#👉step-3-check-the-status-of-the-pods)
    4. [👉Step 4: Check the status of the Longhorn system](#👉step-4-check-the-status-of-the-longhorn-system)
    5. [👉Step 5: Get the storage class](#👉step-5-get-the-storage-class)
    6. [👉Step 6: Create a basic authentication file](#👉step-6-create-a-basic-authentication-file)
    7. [👉Step 7: Create a secret](#👉step-7-create-a-secret)
    8. [👉Step 8: Set up a ingress controller](#👉step-8-set-up-a-ingress-controller)
    9. [👉Step 9: Access the Longhorn UI](#👉step-9-access-the-longhorn-ui)
    10. [👉Step 10: Enable storage replica on worker notes](#👉step-10-enable-storage-replica-on-worker-notes)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to installing `Longhorn` on the servers (nodes) in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the servers (nodes) and the supercluster.

## ✨Steps

### 👉Step 1: Test if the system is ready for Longhorn

```bash
bash <(curl -s https://raw.githubusercontent.com/longhorn/longhorn/refs/tags/v1.7.2/scripts/environment_check.sh)
```

> Let's see some common problems.

- **Problem 1: A kernel module is missing**
```bash
modinfo iscsi_tcp                                   # Check if the module is available
sudo modprobe iscsi_tcp                             # Load the module
echo "iscsi_tcp" | sudo tee -a /etc/modules         # Add the module to the list of modules to load at boot
lsmod | grep iscsi_tcp                              # Check if the module is loaded
```

- **Problem 2: The multipathd service is running**
```bash
sudo systemctl stop multipathd                      # Stop the multipathd service
sudo systemctl disable multipathd                   # Disable the multipathd service
systemctl status multipathd                         # Check the status of the multipathd service
```

- **Problem 3: The nfs-common package is not installed**
```bash
sudo apt-get update && sudo apt-get upgrade -y      # Update and upgrade the system
sudo apt-get install -y nfs-common                  # Install the nfs-common package
dpkg -l | grep nfs-common                           # Check if the nfs-common package is installed
```

### 👉Step 2: Install Longhorn

```bash
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.7.2/deploy/longhorn.yaml
# kubectl delete -f https://raw.githubusercontent.com/longhorn/longhorn/v1.7.2/deploy/longhorn.yaml
# kubectl get namespace longhorn-system -o json > longhorn-system.json
# sudo nano longhorn-system.json
# delete -> "kubernetes" -> "finalizers"
# kubectl replace --raw "/api/v1/namespaces/longhorn-system/finalize" -f longhorn-system.json
```

### 👉Step 3: Check the status of the pods

```bash
kubectl get pods --namespace longhorn-system --watch # OPTIONAL
```

### 👉Step 4: Check the status of the Longhorn system

```bash
kubectl -n longhorn-system get pods # OPTIONAL
```

### 👉Step 5: Get the storage class

```bash
kubectl get storageclass # OPTIONAL
```

### 👉Step 6: Create a basic authentication file

```bash
USER=<USERNAME_HERE>; PASSWORD=<PASSWORD_HERE>; echo "${USER}:$(openssl passwd -stdin -apr1 <<< ${PASSWORD})" >> auth
```

### 👉Step 7: Create a secret

```bash
kubectl create secret generic basic-auth -n longhorn-system --from-file=auth
rm auth
# kubectl delete secret basic-auth -n longhorn-system
```

### 👉Step 8: Set up a ingress controller

```bash
kubectl apply -f https://raw.githubusercontent.com/eliasdhcom/.github/refs/heads/main/K8sInfra/Curiosities/Longhorn/Ingress.yaml
# Service name: longhorn-frontend
# Namespace: longhorn-system
# Port: 8080
```

### 👉Step 9: Access the Longhorn UI

```bash
kubectl get ingress -A
```

### 👉Step 10: Enable storage replica on worker notes

```bash
kubectl label node node01 longhorn.io/target=true
kubectl label node node02 longhorn.io/target=true
kubectl label node node03 longhorn.io/target=true
```

```bash
kubectl label node node01 node-role.kubernetes.io/longhorn=false
kubectl label node node02 node-role.kubernetes.io/longhorn=true
kubectl label node node03 node-role.kubernetes.io/longhorn=true
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
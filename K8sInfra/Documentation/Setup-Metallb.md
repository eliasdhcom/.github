![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Metallb🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Preparation](#👉step-1-preparation)
    2. [👉Step 2: To install MetalLB apply the manifest](#👉step-2-to-install-metallb-apply-the-manifest)
    3. [👉Step 3: Check if the pods are running](#👉step-3-check-if-the-pods-are-running)
    4. [👉Step 4: Set the IP range for the Load Balancer](#👉step-4-set-the-ip-range-for-the-load-balancer)
    6. [👉Step 5: Set the L2 Advertisement](#👉step-6-set-the-l2-advertisement)
    7. [👉Step 6: Check if the L2 Advertisement is set](#👉step-7-check-if-the-l2-advertisement-is-set)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to setting up the `Metallb` on the Kubernetes clusters in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the clusters and the supercluster.

## ✨Steps

### 👉Step 1: Preparation

```bash
kubectl get configmap kube-proxy -n kube-system -o yaml | \
sed -e "s/strictARP: false/strictARP: true/" | \
kubectl apply -f - -n kube-system
```

### 👉Step 2: To install MetalLB apply the manifest

```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
# kubectl delete -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
```

### 👉Step 3: Check if the pods are running

```bash
watch kubectl get pods -n metallb-system # CTRL+C to exit
kubectl api-resources | grep metallb
```

### 👉Step 4: Set the IP range for the Load Balancer

```bash
kubectl apply -f https://raw.githubusercontent.com/eliasdhcom/.github/refs/heads/main/K8sInfra/Curiosities/Metallb/IPAddressPool.yaml
```

### 👉Step 5: Set the L2 Advertisement

```bash
kubectl apply -f https://raw.githubusercontent.com/eliasdhcom/.github/refs/heads/main/K8sInfra/Curiosities/Metallb/L2Advertisement.yaml
```

### 👉Step 6: Check if the L2 Advertisement is set

```bash
kubectl get l2advertisement -n metallb-system
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
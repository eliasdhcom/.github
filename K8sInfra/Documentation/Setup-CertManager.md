![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Cert Manager🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Aplly the latest cert-manager.yaml file on all the clusters](#👉step-1-aplly-the-latest-cert-manageryaml-file-on-all-the-clusters)
    2. [👉Step 2: Check if the pods are running](#👉step-2-check-if-the-pods-are-running)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to setting up the `Cert Manager` on the Kubernetes clusters in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the clusters and the supercluster.

## ✨Steps

### 👉Step 1: Aplly the latest cert-manager.yaml file on all the clusters

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.3/cert-manager.yaml
# kubectl delete -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.3/cert-manager.yaml
```

### 👉Step 2: Check if the pods are running

```bash
watch kubectl get pods -n cert-manager # CTRL+C to exit
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
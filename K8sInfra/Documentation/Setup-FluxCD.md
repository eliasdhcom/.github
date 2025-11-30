![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup FluxCD🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

FluxCD is a powerful GitOps tool that automates the deployment of applications and infrastructure changes to Kubernetes clusters. By using FluxCD, you can ensure that your cluster's state is always in sync with the desired state defined in your Git repository. This guide will walk you through the steps to set up FluxCD on your Kubernetes cluster.

## ✨Steps

- Run this on your machine (where kubectl is configured):
```bash
curl -s https://fluxcd.io/install.sh | sudo bash
```

- Install FluxCD in your cluster:
```bash
flux install
```

- Verify the installation:
```bash
flux check
```

- Bootstrap Flux on your repository:
```bash
flux bootstrap github
    --owner=eliasdhcom
    --repository=gitOps
    --branch=main
    --path=gitops/flux
    --components-extra=image-reflector-controller,image-automation-controller
    --personal
```


## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
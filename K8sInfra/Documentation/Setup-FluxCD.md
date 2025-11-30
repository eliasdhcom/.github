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
flux install --components-extra=image-reflector-controller,image-automation-controller
```

- Verify the installation:
```bash
flux check
```

- Create a deploy key for your Git repository:
```bash
ssh-keygen -t ed25519 -f ~/flux-deploy-key -N "" -C "flux-deploy-key"

cat ~/flux-deploy-key.pub

# Add this public key as a Deploy key on GitHub
# Go to: https://github.com/eliasdhcom/gitOps/settings/keys
# → “Add deploy key”
# Title: flux-deploy-key
# Key: Paste the output from above here
# Allow write access: OFF (uncheck the box!)
# Add key
```

- Bootstrap Flux on your repository:
```bash
flux bootstrap git \
    --url=ssh://git@github.com/eliasdhcom/gitOps.git \
    --branch=main \
    --path=gitops/flux \
    --private-key-file=./flux-deploy-key \
    --components-extra=image-reflector-controller,image-automation-controller

kubectl -n flux-system create secret generic flux-ssh \
    --from-file=identity=$HOME/flux-deploy-key \
    --from-file=identity.pub=$HOME/flux-deploy-key.pub \
    --from-literal=known_hosts="$(ssh-keyscan github.com 2>/dev/null)"

flux reconcile source git gitops -n flux-system
```

- Verify that all components are running:
```bash
flux get all
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
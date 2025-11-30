![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Testing Supercluster🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Problems](#✨problems)
    1. [👉Problem 1: Create yourself an environment to test some network issues.](#👉problem-1-create-yourself-an-environment-to-test-some-network-issues)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document contains some interesting test commands.

## ✨Problems

### 👉Problem 1: Create yourself an environment to test some network issues.

```bash
kubectl run -i --tty --rm network-test --image=debian --restart=Never -- bash

apt-get update && apt upgrade -y
apt-get install dnsutils iputils-ping curl -y

dig eliasdh.com
dig google.com

ping eliasdh.com
ping google.com

curl eliasdh.com
curl google.com

exit
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
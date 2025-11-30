![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Proxy🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Create a new user on the proxy server](#👉step-1-create-a-new-user-on-the-proxy-server)
    2. [👉Step 2: Install Keepalived and Haproxy](#👉step-2-install-keepalived-and-haproxy)
    3. [👉Step 3: Configure the keepalived service](#👉step-3-configure-the-keepalived-service)
    4. [👉Step 4: Create the script to check the API server](#👉step-4-create-the-script-to-check-the-api-server)
    5. [👉Step 5: Make the script executable](#👉step-5-make-the-script-executable)
    6. [👉Step 6: Configure the haproxy service](#👉step-6-configure-the-haproxy-service)
    7. [👉Step 7: Restart the keepalived and haproxy services](#👉step-7-restart-the-keepalived-and-haproxy-services)
    8. [👉Step 8: Check the status of the keepalived and haproxy services](#👉step-8-check-the-status-of-the-keepalived-and-haproxy-services)
    9. [👉Step 9: Test the reverse proxy](#👉step-9-test-the-reverse-proxy)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to setting up a `reverse proxy` on the servers (nodes) in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the servers (nodes) and the supercluster.

## ✨Steps

### 👉Step 1: Create a new user on the proxy server

```bash
sudo useradd -m -s /bin/bash -G sudo user && sudo passwd user
```

### 👉Step 2: Install Keepalived and Haproxy

- [Keepalived website](https://www.keepalived.org/).
- [Haproxy website](https://www.haproxy.org/).

```bash
sudo apt-get install -y keepalived haproxy
```

### 👉Step 3: Configure the keepalived service

```bash
sudo rm -rf /etc/keepalived/keepalived.conf
sudo nano /etc/keepalived/keepalived.conf
```
```conf
global_defs {
    router_id proxy1
    script_user root
    script_security 1
}

vrrp_script check_apiserver {
    script "/etc/keepalived/check_apiserver.sh"
    interval 3
    weight -2
    fall 10
    rise 2
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 101
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.1.170
    }
    track_script {
        check_apiserver
    }
}
```

### 👉Step 4: Create the script to check the API server

```bash
sudo nano /etc/keepalived/check_apiserver.sh
```
```bash
#!/bin/sh
############################
# @author EliasDH Team     #
# @see https://eliasdh.com #
# @since 01/01/2025        #
############################

errorExit() {
    echo "*** $*" 1>&2
    exit 1
}

curl -sfk --max-time 2 https://10.1.0.1:6443/healthz -o /dev/null || errorExit "Error GET https://10.1.0.1:6443/healthz"
curl -sfk --max-time 2 https://10.1.0.2:6443/healthz -o /dev/null || errorExit "Error GET https://10.1.0.2:6443/healthz"
curl -sfk --max-time 2 https://10.1.0.3:6443/healthz -o /dev/null || errorExit "Error GET https://10.1.0.3:6443/healthz"
```

### 👉Step 5: Make the script executable

```bash
sudo chmod +x /etc/keepalived/check_apiserver.sh
```

### 👉Step 6: Configure the haproxy service

```bash
sudo rm -rf /etc/haproxy/haproxy.cfg
sudo nano /etc/haproxy/haproxy.cfg
```
```conf
global
    log stdout format raw local0
    daemon

defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 1
    timeout http-request    10s
    timeout queue           20s
    timeout connect         5s
    timeout client          35s
    timeout server          35s
    timeout http-keep-alive 10s
    timeout check           10s

frontend apiserver
    bind *:6443
    mode tcp
    option tcplog
    default_backend apiserverbackend

backend apiserverbackend
    option httpchk

    http-check connect ssl
    http-check send meth GET uri /healthz
    http-check expect status 200

    mode tcp
    balance     roundrobin
    
    server node01 10.1.0.1:6443 check verify none
    server node02 10.1.0.2:6443 check verify none
    server node03 10.1.0.3:6443 check verify none
```

### 👉Step 7: Restart the keepalived and haproxy services

```bash
sudo systemctl restart keepalived haproxy
```

### 👉Step 8: Check the status of the keepalived and haproxy services

```bash
sudo systemctl status keepalived haproxy
```

### 👉Step 9: Test the reverse proxy

```bash
nc -v 10.1.0.256 6443
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
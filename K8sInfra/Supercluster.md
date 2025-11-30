![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Supercluster🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [📚Documentation](#📚documentation)
4. [🌌Supercluster Design](#🌌supercluster-design)
    1. [🌌Cluster 01](#🌌cluster-01)
    2. [🌌Cluster 02](#🌌cluster-02)
    3. [🌌Cluster 03](#🌌cluster-03)
5. [🖥️Interesting tools to manage your clusters](#🖥️interesting-tools-to-manage-your-clusters)
6. [📜A must read](#📜a-must-read)
7. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides an overview of the supercluster infrastructure, including its design, documentation, and useful tools for managing the clusters within the supercluster. The supercluster consists of multiple Kubernetes clusters, each with its own set of nodes and configurations.

## 📚Documentation

- Setup Supercluster:
    1. [Setup The Operating System For The Nodes](Documentation/Setup-OS.md).
    2. [Setup The Hypervisor For The Nodes](Documentation/Setup-Docker.md).
    3. [Setup Kubernetes](Documentation/Setup-Kubernetes.md).
    4. [Setup The Proxy Server For The Master Nodes](Documentation/Setup-Proxy.md).
    5. [Setup The Clusters For The Supercluster](Documentation/Setup-Clusters.md).
    6. [Setup The Kubernetes Secrets](Documentation/Setup-Secrets.md).

- Setup Cluster Infrastructure:
    1. [Setup Cert Manager](Documentation/Setup-CertManager.md).
    2. [Setup Metallb](Documentation/Setup-Metallb.md).
    3. [Setup Nginx Ingress](Documentation/Setup-NginxIngress.md).
    4. [Setup Longhorn](Documentation/Setup-Longhorn.md).
    5. [Setup FluxCD](Documentation/Setup-FluxCD.md).

- Testing: [Testing The Supercluster](Documentation/Testing-Supercluster.md).

## 🌌Supercluster Design

### 🌌Cluster 01

- Nodes:
    | ID  | Name    | Cluster   | Roll   | IP         | CPU | RAM   | Disk | OS               |
    | --- | ------- | --------- | ------ | ---------- | --- | ----- | -----| -----------------|
    | 000 | proxy1  | cluster01 | ...... | 10.1.0.256 | 1   | 1GB   | 0KB  | Ubuntu 24.04 LTS |
    | 001 | node01  | cluster01 | Master | 10.1.0.1   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 002 | node02  | cluster01 | Master | 10.1.0.2   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 003 | node03  | cluster01 | Master | 10.1.0.3   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 004 | node04  | cluster01 | Worker | 10.1.0.4   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 005 | node05  | cluster01 | Worker | 10.1.0.5   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 006 | node06  | cluster01 | Worker | 10.1.0.6   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 007 | node07  | cluster01 | Worker | 10.1.0.7   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 008 | node08  | cluster01 | Worker | 10.1.0.8   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 009 | node09  | cluster01 | Worker | 10.1.0.9   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | ... | ......  | ......... | ...... | .......... | ... | ....  | .... | ................ |
    | 099 | node99  | cluster01 | Worker | 10.1.0.99  | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |

### 🌌Cluster 02

- Nodes:
    | ID  | Name    | Cluster   | Roll   | IP         | CPU | RAM   | Disk | OS               |
    | --- | ------- | --------- | ------ | ---------- | --- | ----- | -----| -----------------|
    | 100 | proxy1  | cluster02 | ...... | 10.2.0.256 | 1   | 1GB   | 0KB  | Ubuntu 24.04 LTS |
    | 101 | node01  | cluster02 | Master | 10.2.0.1   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 102 | node02  | cluster02 | Master | 10.2.0.2   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 103 | node03  | cluster02 | Master | 10.2.0.3   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 104 | node04  | cluster02 | Worker | 10.2.0.4   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 105 | node05  | cluster02 | Worker | 10.2.0.5   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 106 | node06  | cluster02 | Worker | 10.2.0.6   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 107 | node07  | cluster02 | Worker | 10.2.0.7   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 108 | node08  | cluster02 | Worker | 10.2.0.8   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 109 | node09  | cluster02 | Worker | 10.2.0.9   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | ... | ......  | ......... | ...... | .......... | ... | ....  | .... | ................ |
    | 199 | node99  | cluster02 | Worker | 10.2.0.99  | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |

### 🌌Cluster 03

- Nodes:
    | ID  | Name    | Cluster   | Roll   | IP         | CPU | RAM   | Disk | OS               |
    | --- | ------- | --------- | ------ | ---------- | --- | ----- | -----| -----------------|
    | 200 | proxy1  | cluster03 | ...... | 10.3.0.256 | 1   | 1GB   | 0KB  | Ubuntu 24.04 LTS |
    | 201 | node31  | cluster03 | Master | 10.3.0.1   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 202 | node32  | cluster03 | Master | 10.3.0.2   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 203 | node33  | cluster03 | Master | 10.3.0.3   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 204 | node34  | cluster03 | Worker | 10.3.0.4   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 205 | node35  | cluster03 | Worker | 10.3.0.5   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 206 | node36  | cluster03 | Worker | 10.3.0.6   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 207 | node37  | cluster03 | Worker | 10.3.0.7   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 208 | node38  | cluster03 | Worker | 10.3.0.8   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | 209 | node39  | cluster03 | Worker | 10.3.0.9   | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |
    | ... | ......  | ......... | ...... | .......... | ... | ....  | .... | ................ |
    | 299 | node99  | cluster03 | Worker | 10.3.0.99  | 16  | 64GB  | 8TB  | Ubuntu 24.04 LTS |

## 🖥️Interesting tools to manage your clusters

1. [K9s](Documentation/Setup-K9s.md).
2. [K10s](https://github.com/eliasdehondt/k10s).

## 📜A must read

1. [Github Repository - Kubeadm Load Balancing](https://github.com/kubernetes/kubeadm/blob/main/docs/ha-considerations.md#options-for-software-load-balancing).

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
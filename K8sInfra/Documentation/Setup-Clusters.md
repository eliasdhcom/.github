![logo](https://eliasdh.com/assets/media/images/logo-github.png)
# 💙🤍Setup Clusters🤍💙

## 📘Table of Contents

1. [📘Table of Contents](#📘table-of-contents)
2. [🖖Introduction](#🖖introduction)
3. [✨Steps](#✨steps)
    1. [👉Step 1: Initialize the Kubernetes cluster on the master node](#👉step-1-initialize-the-kubernetes-cluster-on-the-master-node)
    2. [👉Step 2: Copy the credentials to the user's home directory](#👉step-2-copy-the-credentials-to-the-users-home-directory)
    3. [👉Step 3: Join the master nodes to the cluster](#👉step-3-join-the-master-nodes-to-the-cluster)
    4. [👉Step 4: Join the worker nodes to the cluster](#👉step-4-join-the-worker-nodes-to-the-cluster)
    5. [👉Step 5: Install helm](#👉step-5-install-helm)
    6. [👉Step 6: Install the Cilium CNI plugin](#👉step-6-install-the-cilium-cni-plugin)
    7. [👉Step 7: Now wait for all the pots to be running](#👉step-7-now-wait-for-all-the-pots-to-be-running)
    8. [👉Step 8: Check the status of the nodes](#👉step-8-check-the-status-of-the-nodes)
4. [🔗Links](#🔗links)

---

## 🖖Introduction

This document provides a step-by-step guide to setting up the `Kubernetes` clusters in the supercluster. The steps outlined in this document are essential for ensuring the proper functioning of the clusters and the supercluster.

## ✨Steps

### 👉Step 1: Initialize the Kubernetes cluster on the master node

```bash
sudo kubeadm init --config https://raw.githubusercontent.com/eliasdhcom/.github/refs/heads/main/K8sInfra/ClusterConfiguration.yaml --upload-certs --v "5"
```

> **Note:** Copy the kubeadm join commands for the worker nodes and the master node

> Or you can use the following command to get the join command:

```bash
sudo kubeadm init --control-plane-endpoint "192.168.111.201:6443" --pod-network-cidr "10.244.0.0/16" --cri-socket "unix:///run/containerd/containerd.sock" 
```

### 👉Step 2: Copy the credentials to the user's home directory 

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/
```

> **Note:** This most be done on all the master nodes

### 👉Step 3: Join the master nodes to the cluster

```bash
# Cluster01
sudo kubeadm join 10.1.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash> \
        --control-plane --certificate-key <key>

# Cluster02
sudo kubeadm join 10.2.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash> \
        --control-plane --certificate-key <key>

# Cluster03
sudo kubeadm join 10.3.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash> \
        --control-plane --certificate-key <key>
```

### 👉Step 4: Join the worker nodes to the cluster

```bash
# Cluster01
sudo kubeadm join 10.1.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash>

# Cluster02
sudo kubeadm join 10.2.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash>

# Cluster03
sudo kubeadm join 10.3.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash>
```

### 👉Step 5: Install helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
sudo ./get_helm.sh
```

### 👉Step 6: Install the Cilium CNI plugin

```bash
# Install the Cilium CLI
helm repo add cilium https://helm.cilium.io/

# Set the Cilium CLI version
CILIUM_CLI_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/cilium-cli/main/stable.txt)

# Set the Cilium CLI architecture
CLI_ARCH=amd64

# Check the architecture
if [ "$(uname -m)" = "aarch64" ]; then CLI_ARCH=arm64; fi

# Download the Cilium CLI
curl -L --fail --remote-name-all https://github.com/cilium/cilium-cli/releases/download/${CILIUM_CLI_VERSION}/cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}

# Verify the Cilium CLI
sha256sum --check cilium-linux-${CLI_ARCH}.tar.gz.sha256sum

# Extract the Cilium CLI
sudo tar xzvfC cilium-linux-${CLI_ARCH}.tar.gz /usr/local/bin

# Remove the Cilium CLI
rm cilium-linux-${CLI_ARCH}.tar.gz{,.sha256sum}

# Install the Cilium Helm chart
helm install cilium cilium/cilium --version 1.16.1 \
  --namespace kube-system \
  --set prometheus.enabled=true \
  --set operator.prometheus.enabled=true
```

> **Note:** CNI is a plugin that allows Kubernetes to communicate with the network. Cilium is a CNI plugin that provides networking, security, and observability features.

> CNI = Container Network Interface

### 👉Step 7: Now wait for all the pots to be running

```bash
watch kubectl get pods -n kube-system # Press Ctrl+C to exit
```

### 👉Step 8: Set up custom DNS records

```bash
kubectl apply -f https://raw.githubusercontent.com/eliasdhcom/.github/refs/heads/main/K8sInfra/Coredns/ConfigMap.yaml

kubectl rollout restart deployment coredns -n kube-system
```

> **Note:** sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --control-plane-endpoint="192.168.111.50:6443" --v=5

### 👉Step 9: Label the nodes

```bash
kubectl label nodes node01 role=control-plane
kubectl label nodes node02 role=control-plane
kubectl label nodes node03 role=control-plane
kubectl label nodes node04 role=compute
kubectl label nodes node05 role=compute
kubectl label nodes node06 role=compute
kubectl label nodes node07 role=compute
kubectl label nodes node08 role=compute
kubectl label nodes node09 role=compute
```

```bash
kubectl get nodes --show-labels
```

### 👉Step 10: Check the status of the nodes

```bash
watch kubectl get nodes -o wide # Press Ctrl+C to exit
```

## 🔗Links
- 👯 Web hosting company [EliasDH.com](https://eliasdh.com).
- 📫 How to reach us info@eliasdh.com
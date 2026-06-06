#!/bin/bash

# Kubernetes Worker Node Install Script
# Ubuntu 24.04 / 22.04
# Run as root:
# sudo bash install-k8s-worker.sh

set -e

echo "===== Updating system ====="
apt update && apt upgrade -y

echo "===== Disabling swap ====="
swapoff -a
sed -i '/ swap / s/^/#/' /etc/fstab

echo "===== Loading kernel modules ====="
cat <<EOF | tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

echo "===== Setting sysctl params ====="
cat <<EOF | tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sysctl --system

echo "===== Installing dependencies ====="
apt install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gpg \
    containerd

echo "===== Configuring containerd ====="
mkdir -p /etc/containerd

containerd config default | tee /etc/containerd/config.toml

sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' \
/etc/containerd/config.toml

systemctl restart containerd
systemctl enable containerd

echo "===== Adding Kubernetes repo ====="
mkdir -p /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | \
gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | \
tee /etc/apt/sources.list.d/kubernetes.list

echo "===== Installing Kubernetes ====="
apt update

apt install -y kubelet kubeadm kubectl

apt-mark hold kubelet kubeadm kubectl

systemctl enable kubelet

echo ""
echo "========================================="
echo "Worker node setup complete"
echo "========================================="
echo ""
echo "Run the JOIN command from your master node:"
echo ""
echo "kubeadm join <MASTER-IP>:6443 --token <TOKEN> \\"
echo "    --discovery-token-ca-cert-hash sha256:<HASH>"
echo ""
echo "To generate a new join command on master:"
echo ""
echo "kubeadm token create --print-join-command"

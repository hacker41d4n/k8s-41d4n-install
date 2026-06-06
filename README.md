# k8s-41d4n-install
My scripts to install master and workers


## execute Master script
chmod +x k8s-master.sh
sudo ./k8s-master.sh

## Init 

sudo kubeadm init --pod-network-cidr=10.244.0.0/16

## execute Worker script
chmod +x k8s-worker.sh
sudo ./k8s-worker.sh


## Get nodes
Your Kubernetes cluster is running, but kubectl is not configured for your user yet.

Run these commands on the master node EXACTLY:

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

Then test:

kubectl get nodes

## Flannel
1. STOP using sudo with kubectl

Run:

kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
2. If you insist on sudo (not recommended), force kubeconfig:
sudo KUBECONFIG=$HOME/.kube/config kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
3. Verify kubeconfig is correct

Run:

echo $KUBECONFIG

Then:

ls -l $HOME/.kube/config

If missing, fix it again:

mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
🚀 After Flannel installs

Check:

kubectl get pods -A

## Deploy services

kubectl apply -f homelab.yaml


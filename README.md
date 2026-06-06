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

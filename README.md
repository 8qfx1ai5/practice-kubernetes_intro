# k8s-kubeadm-tutorial
tutorial to show how to setup a minimum viable Kubernetes cluster with kubeadm

## Before you begin

This tutorial is going to use digitalocean as a cloud computing platform, which will provide us with 2 or 3 nodes to host our kubernetes cluster on.

This tutorial should be interchangeable with other cloud native solutions (e.g. hetzner cloud).

While automation is baked into my being this tutorial will do everything manually, as well documented as possible to give an step by step insight on how to setup such a cluster.
Obviosly one of the next steps after going through this tutorial should be to automate these steps by useig tools such as e.g. ansible. But this is not part of this tutorial.

We are going to create a minimum viable Kubernetes cluster on 3 digitalocean vm instances (droplets).

We are also going to use base droplets to see each step (e.g. installing docker).

Pls read [this](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#before-you-begin) to to see which kind of vm's and requirements we are going to need.

The cluster will consist of 2 or 3 nodes, one master and one or two worker nodes
node0 = master node
node1 = worker node
(node2 = worker node) (optional)

## Objectives

____

## Instructions

### we need ssh-keys
``` bash
mkdir -p ~/.ssh
cd ~/.ssh
ssh-keygen -b 2048 -t rsa -N '' -f k8s-kubeadm-test
ssh-add k8s-kubeadm-test
cat k8s-kubeadm-test.pub | xclip -selection clipboard
```

### we need vms
- create 3 vms with ubuntu > 16 | + 2gb ram
- ensure / put ssh-keys on the machine
- copy ips


### ssh into all vms
``` bash
ssh root@95.216.188.175
ssh root@95.216.188.176
ssh root@95.216.188.177
```

### install docker on all machienes
- changed docker version from 17 to 18 bec 17 not found on ubuntu 18
- run on all

``` bash
apt-get update
apt-get install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable"
# apt-get update && apt-get install -y docker-ce=$(apt-cache madison docker-ce | grep 17.03 | head -1 | awk '{print $3}')
apt-get update && apt-get install -y docker-ce=$(apt-cache madison docker-ce | grep 18.06.1 | head -1 | awk '{print $3}')
```

### install kubeadm on all machienes
- run on all

``` bash
apt-get update && apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```

### preperation for pod-network
- run on all

``` bash
sysctl net.bridge.bridge-nf-call-iptables=1
```

### disable swap
- run on all

``` bash
swapoff -a
sed -i '/ swap / s/^/#/' /etc/fstab
```

### create master
- run on MASTER ONLY

``` bash
kubeadm init --ignore-preflight-errors=cri
mkdir -p $HOME/.kube/; cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

### join worker nodes
- run on WORKER nodes
- the following command is an EXAMPLE command, copy the true command from the output of the `kubeadm init --ignore-preflight-errors=cri`

``` bash
# EXAMPLE ... copy the original command from the output of the `kubeadm init --ignore-preflight-errors=cri` command
kubeadm join 95.216.188.168:6443 --token q65wgj.6rockbbb6q5b20fk --discovery-token-ca-cert-hash sha256:e597db683f78ba3a0ce545fec0cbf778da8e2c2107c3ad2a31ddf2f1d8745e89
```

### check nodes
- run on MASTER ONLY

``` bash
kubectl get nodes
```

### install pod-network
- run on MASTER ONLY

``` bash
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

### deploy sock shop
- run on MASTER ONLY
``` bash
git clone https://github.com/microservices-demo/microservices-demo.git
kubectl create namespace sock-shop
kubectl apply -f microservices-demo/deploy/kubernetes/complete-demo.yaml
```

### check sock shop
- run on MASTER ONLY

``` bash
kubectl -n sock-shop get pods

# check nodeport
kubectl -n sock-shop get svc
```
____

## Tear down

## What’s next
- https://www.weave.works/docs/scope/latest/installing/#k8s
- docker registry
- gitlab-ci
- example pipeline

## Feedback

## resources
based on:
- https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-1-10-cluster-using-kubeadm-on-ubuntu-16-04
- https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm
- https://kubernetes.io/docs/setup/independent/install-kubeadm/
- https://kubernetes.io/docs/setup/independent/install-kubeadm/#check-required-ports
- https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network
- https://www.youtube.com/watch?v=b_fOIELGMDY

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

## Instructions

## Tear down

## What’s next

## Feedback

## resources
based on:
- https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-1-10-cluster-using-kubeadm-on-ubuntu-16-04
- https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm
- https://kubernetes.io/docs/setup/independent/install-kubeadm/
- https://kubernetes.io/docs/setup/independent/install-kubeadm/#check-required-ports
- https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network

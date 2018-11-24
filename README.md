# Introduction
  This book is a step-by-step handbook to build a high available kubernetes cluster. Although there are many tools that can help us to create kubernetes cluster, but here I still use create this cluster manually. from this way, I will have a better understading of the cluster components and other addons and plugins. This test k8s cluster will simulate the scenario that integrate a existing network with k8s cluster, make pods are accessible to formally created servers.


# Cluster Design

## 1.  Network diagram


 
|Node|IP address(k8s) | role| version|
|------------|----------------|------|-----|
|kube-master1| 192.168.136.132|master| 1.12|
|kube-master2| 192.168.136.133|master| 1.12|
|kube-master3| 192.168.136.134|master| 1.12|
|kube-node1| 192.168.136.135|work-node| 1.12|
|kube-node2| 192.168.136.136|work-node| 1.12|



## 2. K8S POD/SVC Ntworks

- Pod: 10.30.0.0/16
- service: 10.40.0.0/16
- loadbalancer: 10.50.0.0/16

## 3. Other components

|component|version|
|---------|-------|
|Docker CE |18.06|
|etcd| 3.2.24|
|cilium|1.3.0|
|calico|3.3.1|
|kube-router|0.2.3|
|metallb|v0.7.3|
## 4. OS 

ubuntu 18.04 

## 5. BGP router
Quagga:  
- separate Linux host running as a router
- can work with metallb to assign the loadbalancer IP for services
- also can work with kube-router to expose POD/SVC IP
- also can work with kube-router to  add external IP for service

# Cluster Deployment

1. All kubernetes master components can be deployed etheir in binary or pod mode
2. Master nodes disable the workload
3. Use IPVS for service proxy(kube-router or kube-proxy)
4. Use NFS as the storage backend  

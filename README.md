# K8S_kind

### Kubernetes Installation Guide

<p align="center">
  <img src="https://d33wubrfki0l68.cloudfront.net/d0c94836ab5b896f29728f3c4798054539303799/9f948/logo/logo.png" height = 250 width = 400>
</p>

```text
Kind 為一個工具，能幫助使用者建立一個 'containerized' Kubernetes clutser，且所有的節點皆為 'containerized' nodes
kubectl is a tool to manage your kubernetes cluster including deployment of containers and services
```

### 安裝 Kind
```Bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.18.0/bin/linux/amd64/kubectl

chmod +x ./kubectl

mv kubectl /usr/bin
```

### 安裝 kubectl
```Bash
wget https://kind.sigs.k8s.io/dl/v0.8.1/kind-$(uname)-amd64

mv kind-Linux-amd64 ./kind                # rename 

chmod 777 ./kind                          # make kind executable

mv kind /usr/bin/
```
### 建立 Kubernetes Cluster
```Bash
kind create cluster --name name-of-your-cluster   # 第一次運行會 pull image of kindest/node(1.6GB) from docker hub 
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/create_cluster.png" width="750" height="300"></img>

#### Check your clusters use kubectl
```Bash
kubectl cluster-info --context kind-name-for-your-cluster  # For example: kind-test
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/cluster_info.png"  width="750" ></img>

#### Check your clusters built by kind
```Bash
kind get clusters
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/get_clusters.png"></img>

#### Check the node of your cluster (just one which is master)
```Bash
kubectl get nodes --context kind-name-of-your-cluster # For example: kind-test
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/node.png"></img>

#### To delete your cluster
##### `Actually, just delete all the container of nodes (including master)`
```Bash
kind delete cluster --name name-of-your-cluser # For example: kind delete cluster --name test
```

### How to create Cluster with multi nodes
#### Create a .yaml file first
```Bash
vi name-of-your-config.yaml
```
##### Content would be like
```yaml
(three nodes cluster)
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: kindest/node:v1.16.9@sha256:7175872357bc85847ec4b1aba46ed1d12fa054c83ac7a8a11f5c268957fd5765  # imgae of your node

- role: worker
  image: kindest/node:v1.16.9@sha256:7175872357bc85847ec4b1aba46ed1d12fa054c83ac7a8a11f5c268957fd5765

- role: worker
  image: kindest/node:v1.16.9@sha256:7175872357bc85847ec4b1aba46ed1d12fa054c83ac7a8a11f5c268957fd5765 
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/multi_nodes.png"></img>

#### Check the the nodes of your cluster you just created
```Bash
kubectl get nodes
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/multi_nodes2.png"></img>

### Deploy your container into cluster
```text
施工中
```
<img src="https://i.pinimg.com/originals/26/ac/06/26ac065e9899c74f360ccfbd351108ad.jpg" height = 500 width = 500></img>

# K8S_kind

### Kubernetes Installation Guide

<p align="center">
  <img src="https://d33wubrfki0l68.cloudfront.net/d0c94836ab5b896f29728f3c4798054539303799/9f948/logo/logo.png" height = 250 width = 400>
</p>

```text
Kind(Kubernetes inside docker) 為一個工具，能幫助使用者建立一個 'containerized' Kubernetes clutser，且所有的節點皆為 'containerized' nodes
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
### 1.建立 Kubernetes Cluster
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
----
### 2.How to create Cluster with multi nodes
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


----
### 3.Deploy your container into cluster
#### To deploy your app to cluster
```Bash
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 # --image image-of-your-container-to-deploy 
```

#### To check your deployments
```Bash
kubectl get deployments

kubectl describe deployments
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/get_deployments.png">


#### Check your container is on which node
```
kubectl get pods         # get the name of your pods 
kubectl describe pods
```
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/pods.png">


#### How to get to the application inside your container (Docker inside Docker)
* Use Service object
```
kubectl expose deployment/kubernetes --type='NodePort' --port 8080

kubectl describe service/kubernetes-bootcamp
```

* you can see that Port: 8080/TCP is the port inside your container and NodePort: 31995/TCP is port of your physical node port mapped with container port
<img src="https://github.com/LinShien/K8S_kind/blob/master/images/service.png">


* Now you can access your app with NodeIP:NodePort
* To get the NodeIP where your container is deployed

```Bash
kubectl describe pods kubernetes-bootcamp | grep -i node
```
<img src = "https://github.com/LinShien/K8S_kind/blob/master/images/NodeIP.png">


```Bash
curl 172.18.0.3:31995
```
* Congrats !!!
<img src = "https://github.com/LinShien/K8S_kind/blob/master/images/result.png">

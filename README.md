# Kubernetes Tools and Utilities 

![image](https://user-images.githubusercontent.com/18049790/43352583-0b37edda-9269-11e8-9695-1e8de81acb76.png)

## Disclaimer
* Opinions expressed here are solely my own and do not express the views or opinions of JPMorgan Chase.
* Any third-party trademarks are the intellectual property of their respective owners and any mention herein is for referential purposes only. 

## Table of Contents

1. Kubernetes Command Line Tools
  * 1.1 What is `kubectl`?
  * 1.2 Install `kubectl`
  * 1.3 `kubectx`
  * 1.3 `kubens`
  * 1.4 `kube-ps1`
2. Helm 3 
  * 2.1 What is Helm?
  * 2.2 Install Helm 3
3. kubectl top
* 3.1 What is `kubectl top`?
* 3.1 Install `kubectl top`
4. Kubernetes Web Tools
  * 4.1 What is Octant
  * 4.2 Install Octant
  * 4.3 Start Octant
5. Skaffold
  * 5.1 What is Skaffold
  * 5.2 Install Skaffold
6. Optional Kubernetes Tools 
  * 6.1 krew
  * 6.2 kubectl tree (krew plugin)
  * 6.3 kube-score

## 1. Kubernetes Command Line Tools 

### 1.1 What is kubectl?

`kubectl` is a command line tool used to interact with any Kubernetes clusters.

In the diagram below you see `kubectl` interacts with the Kubernetes API Server.

![image](https://user-images.githubusercontent.com/18049790/65854426-30332f00-e38f-11e9-89a9-b19cc005db91.png)
Credit to [What is Kubernetes](https://www.learnitguide.net/2018/08/what-is-kubernetes-learn-kubernetes.html)

### 1.2 Install kubectl
```
cd ~/ && rm -R ~/kubectl
cd ~/ && mkdir kubectl && cd kubectl
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

### 1.3 [kubectx & kubens](https://github.com/ahmetb/kubectx) 
* `kubectx` - switch between clusters back and forth
* `kubens` - switch between Kubernetes namespaces smoothly

#### Install `kubectx` and `kubens`

`sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx`

`sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx`

`sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens`


### 1.4 [kube-ps1](https://github.com/jonmosco/kube-ps1)
* kube-ps1 - Kubernetes prompt info for bash

#### Install kube-ps1
```
sudo git clone https://github.com/jonmosco/kube-ps1.git /opt/kube-ps1
```

Source the kube-ps1.sh in your ~/.bashrc

To do this, update your .bashrc and append the following lines:

`vi ~/.bashrc`
```
KUBE_PS1_SYMBOL_ENABLE=false
source /opt/kube-ps1/kube-ps1.sh
PS1='[\u@\h \w $(kube_ps1)]\$ '
```

And reinitialize your shell with `. ~/.bashrc`

I put this environment variable in `KUBE_PS1_SYMBOL_ENABLE=false` as the Kubernetes symbol did not display correctly using my font.

You can check if your terminal font supports the Kubernetes symbol with this command `echo $'\u2388'`

## 2. Helm 3

### 2.1 What is Helm?
* [Helm](https://helm.sh/) is a package manager for Kubernetes

### 2.2 Install Helm 3

```
cd ~/ && rm -R ~/helm-3
cd ~/ && mkdir helm-3 && cd helm-3
wget https://get.helm.sh/helm-v3.2.4-linux-amd64.tar.gz
tar -zxvf helm-v3.2.4-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

## 3. kubectl top 

### 3.1 What is kubectl top? (Kubernetes version of Linux `top` or `htop`)

`kubectl top` displays Resource (CPU/Memory/Storage) usage.

`kubectl top` depends on [metrics-server](https://github.com/kubernetes-sigs/metrics-server)

Clarification between [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) and [metrics-server](https://github.com/kubernetes-sigs/metrics-server) 

* kube-state-metrics - kube-state-metrics is a simple service that listens to the Kubernetes API server and generates metrics about the *state of the objects* 
* metrics-server - metrics server is a scalable, efficient source of *container resource metrics* for Kubernetes built-in autoscaling pipelines.

Horizontal Pod Autoscaler and Vertical Pod Autoscaler also depend on [metrics-server](https://github.com/kubernetes-sigs/metrics-server) for metrics to scale pods as required.

`kubectl top` does not work out of the box on Docker Desktop on Windows

### 3.2 Install kubectl top

Please follow these steps to enable `kubectl top` on Docker for Desktop on Windows

`kubectl create namespace ns-metrics-server`

`kubectl apply -f "https://raw.githubusercontent.com/jamesbuckett/kubernetes-tools/master/components.yaml"`

Wait a few minutes for metrics to become available.

Change to a namespace with running pods.

Test that `kubectl top` is working:
* `kubectl top nodes`

```
[i725081@surface-2 ~ (docker-desktop:default)]$ kubectl top node
NAME             CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
docker-desktop   815m         10%    2016Mi          15%
```

* `kubectl top pod`

```
[i725081@surface-2 ~ (docker-desktop:kube-system)]$ kubectl top pods
NAME                                     CPU(cores)   MEMORY(bytes)
coredns-5644d7b6d9-f8c2c                 11m          12Mi
coredns-5644d7b6d9-fsrfh                 9m           11Mi
etcd-docker-desktop                      56m          34Mi
kube-apiserver-docker-desktop            227m         330Mi
kube-controller-manager-docker-desktop   73m          40Mi
kube-proxy-b9pgg                         2m           16Mi
kube-scheduler-docker-desktop            5m           14Mi
storage-provisioner                      10m          12Mi
vpnkit-controller                        0m           20Mi
```

## 4. Kubernetes Web Tools

### 4.1 What is Octant 
* [Octant](https://github.com/vmware-tanzu/octant) is a web-based highly extensible platform for developers to better understand the complexity of Kubernetes clusters.

### 4.2 Install Octant (Linux)
```
cd ~/ && rm -R ~/octant
cd ~/ && mkdir octant && cd octant
curl -LO https://github.com/vmware-tanzu/octant/releases/download/v0.14.0/octant_0.14.0_Linux-64bit.tar.gz
tar -xvf octant_0.14.0_Linux-64bit.tar.gz
sudo mv ./octant_0.14.0_Linux-64bit/octant /usr/local/bin/octant
```

### Start Octant (Remote Linux Instance)

If Octant is installed local, just run `octant -n <namespace>`

If octant is installed on a remote Linux instance then follow instructions below.

Obtain the external IP (Public IPv4) address of the Linux instance running Octant

If you are following the [microservices-metrics-chaos](https://github.com/jamesbuckett/microservices-metrics-chaos) tutorial use this command to export the Public IP of `digital-ocean-droplet`

```
cd ~
DROPLET_ADDR=$(doctl compute droplet list | awk 'FNR == 2 {print $3}')
export $DROPLET_ADDR
echo "export DROPLET_ADDR=$DROPLET_ADDR" >> ~/.bashrc
echo "export OCTANT_ACCEPTED_HOSTS=$DROPLET_ADDR" >> ~/.bashrc
echo "export OCTANT_DISABLE_OPEN_BROWSER=1" >> ~/.bashrc
echo "export OCTANT_LISTENER_ADDR=0.0.0.0:8900" >> ~/.bashrc
. ~/.bashrc
clear
printf "%s\n"  "The URL for Octant is: http://$DROPLET_ADDR:8900"
```

In a new terminal start Octant, to prevent Octant flooding the terminal with messages:
```
octant &
```

```diff
- All tool after this point are optional, all tools above this point are required for the tutorials -
```

## 5. Skaffold

### 5.1 What is Skaffold
* [Skaffold](https://skaffold.dev) is a Continuous Delivery capability for Kubernetes

### 5.2 Install Skaffold

```
cd ~/ && rm -R ~/skaffold
cd ~/ && mkdir skaffold && cd skaffold
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64
sudo install skaffold /usr/local/bin/
```

## 6. Optional Kubernetes Tools

### 6. 1 [krew](https://github.com/kubernetes-sigs/krew)

#### What is krew
* krew is a tool that makes it easy to use kubectl [plugins](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/). 
* krew helps you discover plugins, install and manage them on your machine. 
* It is similar to tools like apt, dnf or brew. Today, over 70 kubectl plugins are available on krew.

#### Install krew (Linux) 

```
(
  set -x; cd "$(mktemp -d)" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/download/v0.3.3/krew.{tar.gz,yaml}" &&
  tar zxvf krew.tar.gz &&
  KREW=./krew-"$(uname | tr '[:upper:]' '[:lower:]')_amd64" &&
  "$KREW" install --manifest=krew.yaml --archive=krew.tar.gz &&
  "$KREW" update
)
```

Add $HOME/.krew/bin directory to your PATH environment variable. 

To do this, update your .bashrc and append the following line:

`export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"`

and restart your shell.

Verifying installation
* Run `kubectl plugin list`

Upgrading krew
* It can be upgraded like a plugin by running the `kubectl krew upgrade` command.

### 6.2 [kubectl tree](https://github.com/ahmetb/kubectl-tree)
* kubectl plugin to browse Kubernetes object hierarchies as a tree

#### Install kubectl tree
```
kubectl krew install tree
kubectl tree --help
```

#### Use kubectl tree
```
kubectl tree KIND NAME [flags]
kubectl tree deployment my-app
kubectl tree kservice.v1.serving.knative.dev my-app
```

```
[root@digital-ocean-droplet ~ (do-sgp1-digital-ocean-cluster:sock-shop)]# kubectl tree deployments front-end
NAMESPACE  NAME                                READY  REASON  AGE
sock-shop  Deployment/front-end                -              46h
sock-shop  └─ReplicaSet/front-end-5594987df6   -              46h
sock-shop    ├─Pod/front-end-5594987df6-cg87t  True           46h
sock-shop    ├─Pod/front-end-5594987df6-jl4pc  True           46h
sock-shop    ├─Pod/front-end-5594987df6-wg5zg  True           46h
sock-shop    └─Pod/front-end-5594987df6-xfdws  True           46h
```

### 6.3 [kube-score](https://github.com/zegl/kube-score)

#### What is kube-score
* Kubernetes object analysis with recommendations for improved reliability and security

#### Install kube-score
```
cd ~/ && mkdir kube-score && cd kube-score
curl -LO https://github.com/zegl/kube-score/releases/download/v1.4.0/kube-score_1.4.0_linux_amd64.tar.gz
tar -xvf kube-score_1.4.0_linux_amd64.tar.gz
sudo mv ~/kube-score/kube-score /usr/local/bin
```

```
wget https://raw.githubusercontent.com/jamesbuckett/microservices-metrics-chaos/master/complete-demo.yaml

kube-score score complete-demo.yaml
```

```
extensions/v1beta1/Deployment carts in sock-shop                              💥
    [WARNING] Stable version
        · The apiVersion and kind extensions/v1beta1/Deployment is deprecated
            It's recommended to use apps/v1 instead
    [CRITICAL] Container Resources
        · carts -> CPU limit is not set
            Resource limits are recommended to avoid resource DDOS. Set resources.limits.cpu
        · carts -> Memory limit is not set
            Resource limits are recommended to avoid resource DDOS. Set resources.limits.memory
        · carts -> CPU request is not set
            Resource requests are recommended to make sure that the application can start and run without crashing. Set
            resources.requests.cpu
        · carts -> Memory request is not set
            Resource requests are recommended to make sure that the application can start and run without crashing. Set
            resources.requests.memory
    [CRITICAL] Container Image Pull Policy
        · carts -> ImagePullPolicy is not set to Always
            It's recommended to always set the ImagePullPolicy to Always, to make sure that the imagePullSecrets are always correct, and
            to always get the image you want.
    [CRITICAL] Pod NetworkPolicy
        · The pod does not have a matching network policy
            Create a NetworkPolicy that targets this pod
    [CRITICAL] Pod Probes
        · Container is missing a readinessProbe
            Without a readinessProbe Services will start sending traffic to this pod before it's ready
        · Container is missing a livenessProbe
            Without a livenessProbe kubelet can not restart the Pod if it has crashed
    [CRITICAL] Container Security Context
        · carts -> The container is privileged
            Set securityContext.privileged to false
        · carts -> The container running with a low group ID
            A groupid above 10 000 is recommended to avoid conflicts with the host. Set securityContext.runAsGroup to a value > 10000
...
v1/Service carts in sock-shop                                                 ✅
v1/Service carts-db in sock-shop                                              ✅
v1/Service catalogue in sock-shop                                             ✅
v1/Service catalogue-db in sock-shop                                          ✅
v1/Service front-end in sock-shop                                             ✅
v1/Service orders in sock-shop                                                ✅
v1/Service orders-db in sock-shop                                             ✅
v1/Service payment in sock-shop                                               ✅
v1/Service queue-master in sock-shop                                          ✅
v1/Service rabbitmq in sock-shop                                              ✅
v1/Service shipping in sock-shop                                              ✅
v1/Service user in sock-shop                                                  ✅
v1/Service user-db in sock-shop                                               ✅
```

*End of Section*

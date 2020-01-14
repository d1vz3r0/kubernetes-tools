# Useful Kubernetes Tools and Utilities 

![image](https://user-images.githubusercontent.com/18049790/43352583-0b37edda-9269-11e8-9695-1e8de81acb76.png)

## Disclaimer
* Opinions expressed here are solely my own and do not express the views or opinions of JPMorgan Chase.
* Any third-party trademarks are the intellectual property of their respective owners and any mention herein is for referential purposes only. 

## Table of Contents
* Kubernetes Command Line Tools
  * kubectx
  * kubens
  * kube-ps1
* Kubernetes Web Tools (Octent)
  * What is Octent
  * Install Octent
  * Start Octent
* krew
  * What is krew
  * Install krew (linux)
* kubectl tree (krew plugin)
  * What is kubectl tree
  * Install kubectl tree

## Kubernetes Command Line Tools 

### [kubectx & kubens](https://github.com/ahmetb/kubectx) 
* kubectx - switch between clusters back and forth
* kubens - switch between Kubernetes namespaces smoothly

#### Install kubectx and kubens
```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

### [kube-ps1](https://github.com/jonmosco/kube-ps1)
* kube-ps1 - Kubernetes prompt info for bash

#### Install kube-ps1
```
sudo git clone https://github.com/jonmosco/kube-ps1.git /opt/kube-ps1
```

Source the kube-ps1.sh in your ~/.bashrc

To do this, update your .bashrc and append the following lines:

```
KUBE_PS1_SYMBOL_ENABLE=false
source /opt/kube-ps1/kube-ps1.sh
PS1='[\u@\h \w $(kube_ps1)]\$ '
```

and restart your shell.

I put this environment variable in `KUBE_PS1_SYMBOL_ENABLE=false` as the Kubernetes symbol did not display correctly using my font.

You can check if your terminal font supports the Kubernetes symbol with this command `echo $'\u2388'`

### [kube-score](https://github.com/zegl/kube-score)

#### What is kube-score
* Kubernetes object analysis with recommendations for improved reliability and security


## Octent

### What is Octant 
* [Octant](https://github.com/vmware-tanzu/octant) is a web-based highly extensible platform for developers to better understand the complexity of Kubernetes clusters.

### Install Octant (Linux)
```
cd ~/ && mkdir octant && cd octant
curl -LO https://github.com/vmware-tanzu/octant/releases/download/v0.9.1/octant_0.9.1_Linux-64bit.tar.gz
tar -xvf octant_0.9.1_Linux-64bit.tar.gz
sudo mv ./octant_0.9.1_Linux-64bit/octant /usr/local/bin/octant
```

### Start Octant (Remote Linux Instance)

If local installed just run `octant -n <namespace>`

If octant is installed on a remote Linux instance then follow instructions below.

Obtain the external IP (Public IPv4) address of the Linux instance running Octant

Update `.bashrc` will the following lines: 
```
export OCTANT_ACCEPTED_HOSTS=<Public IPv4>
export OCTANT_DISABLE_OPEN_BROWSER=1
OCTANT_LISTENER_ADDR=0.0.0.0:8900 octant &
```

and restart your shell.

Open this URL link to access Octant : `http://<Public IPv4>:8900`

## [krew](https://github.com/kubernetes-sigs/krew)

### What is krew
* krew is a tool that makes it easy to use kubectl [plugins](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/). 
* krew helps you discover plugins, install and manage them on your machine. 
* It is similar to tools like apt, dnf or brew. Today, over 70 kubectl plugins are available on krew.

### Install krew (Linux) 

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

## [kubectl tree](https://github.com/ahmetb/kubectl-tree)
* kubectl plugin to browse Kubernetes object hierarchies as a tree

### Install kubectl tree
```
kubectl krew install tree
kubectl tree --help
```

### Use kubectl tree
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

*End of Section*




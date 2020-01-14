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
* Octent
  * What is Octent
  * Install Octent
  * Start Octent

## Kubernetes Command Line Tools 

### [kubectx & kubens](https://github.com/ahmetb/kubectx) 
* kubectx - switch between clusters back and forth
* kubens - switch between Kubernetes namespaces smoothly

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

### [kube-ps1](https://github.com/jonmosco/kube-ps1)
* kube-ps1 - Kubernetes prompt info for bash

Clone the repository
```
sudo git clone https://github.com/jonmosco/kube-ps1.git /opt/kube-ps1
```

Source the kube-ps1.sh in your ~/.bashrc

```
cd 
vi .bashrc
```

```
KUBE_PS1_SYMBOL_ENABLE=false
source /opt/kube-ps1/kube-ps1.sh
PS1='[\u@\h \w $(kube_ps1)]\$ '
```

```
. .bashrc
```

I put this environment variable in `KUBE_PS1_SYMBOL_ENABLE=false` as the Kubernetes symbol did not display correctly using my font.

You can check if your terminal font supports the Kubernetes symbol with this command `echo $'\u2388'`

## Octent

### What is Octant 
[Octant](https://github.com/vmware-tanzu/octant) is a web-based highly extensible platform for developers to better understand the complexity of Kubernetes clusters.

### Install Octant (Linux)
```
cd ~/ && mkdir octant && cd octant
curl -LO https://github.com/vmware-tanzu/octant/releases/download/v0.9.1/octant_0.9.1_Linux-64bit.tar.gz
tar -xvf octant_0.9.1_Linux-64bit.tar.gz
sudo mv ./octant_0.9.1_Linux-64bit/octant /usr/local/bin/octant
```

### Start Octant

Obtain the external IP (Public IPv4) address of the Linux instance running Octant

```
export OCTANT_ACCEPTED_HOSTS=<Public IPv4>
export OCTANT_DISABLE_OPEN_BROWSER=1
OCTANT_LISTENER_ADDR=0.0.0.0:8900 octant &
```

Open this URL link : `http://<Public IPv4>:8900`

## [krew](https://github.com/kubernetes-sigs/krew)

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

Run `kubectl plugin list`

Upgrading krew

It can be upgraded like a plugin by running the `kubectl krew upgrade` command.

*End of Section*




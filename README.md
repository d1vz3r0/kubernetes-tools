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

### Kubernetes Command Line Tools 

#### [kubectx & kubens](https://github.com/ahmetb/kubectx) 
* kubectx - switch between clusters back and forth
* kubens - switch between Kubernetes namespaces smoothly

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

#### [kube-ps1](https://github.com/jonmosco/kube-ps1)
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




End of Section

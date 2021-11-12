<h1>K3S cluster on Raspberry Pi</h1>

This script prepares host Raspberry Pis for K3s Installation.

```
OS: Ubuntu 20.04.3 LTS
```

The script does the following:
- Assigns the hostname according to the hosts.yaml file.
- Disables swap for the current session.
- Disables swap permanently.
- Updates all Apt packages.
- Reboots all nodes.

```
ansible-playbook -i hosts.yaml prepcluster.yaml
```

K3S can be installed using the k3s-ansible scripts.
The version of k3s must be changed to v1.22.3+k3s1 in inventory/my-cluster/group_vars/all.yaml.

Longhorn can be easily installed using helm:

```
# add the longhorn helm repository
helm repo add longhorn https://charts.longhorn.io
# update helm repos
helm repo update
# create the namespace
kubectl create namespace longhorn-system
# install longhorn
helm install longhorn longhorn/longhorn --namespace longhorn-system
```

Ingress

```
# Install arkade, an awesome tool for installing kubernetes resources
curl -sLS https://get.arkade.dev | sudo sh
# Confirm arkade has been installed correctly
arkade version
# Use arkade to install nginx ingress
arkade install ingress-nginx --namespace default
# Use arkade to install certificate manager
arkade install cert-manager
```

Parts of theses instructions were taken from the following blog post by Nima Mahmoudi.
https://itnext.io/the-ultimate-guide-to-building-your-personal-k3s-cluster-bf2643f31dd3

# Recipe for installing and configuring:
## ansible, k3s, helm, minecraft, ingress-nginx
## on fresh Ubuntu 20.04.2 LTS 32 bit 
## on minimum 2 Raspberry Pi 4B
## from Windows WSL2 (Ubuntu) 

## 0. For better cli experience fish shell recommended
```
sudo apt-get install fish

fish
```

## 1. Install ansible

```
sudo apt-get install ansible -y
```

## 2. Install and configure k3s

```
git clone https://github.com/rancher/k3s-ansible

cd k3s-ansible

vi inventory/hosts.ini

vi inventory/groups_vars/all.yml

ansible-playbook site.yml -i inventory/hosts.ini 
```
note: to fetch kubeconfig ssh into master node
copy kubeconfig contents and paste to local
WSL Ubuntu environment.

## 3. Install and configure helm

```
wget https://get.helm.sh/helm-v3.5.4-linux-amd64.tar.gz

tar -zxvf helm-v3.5.4-linux-amd64.tar.gz

mv linux-amd64/helm /usr/local/bin/helm
```

## 4. Install minecraft

```
helm repo add itzg https://itzg.github.io/minecraft-server-charts/

helm repo update

kubectl create namespace minecraft

helm install --namespace minecraft minecraft -f values.yaml itzg/minecraft

```

## 5. Install ingress-nginx

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --set tcp.25565="minecraft/minecraft-minecraft:25565"
```


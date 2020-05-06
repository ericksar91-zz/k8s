# K8S

### INSTALL KUBECTL
Install [Link](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux)

### INSTALL MINIKUBE
Install [Link](https://kubernetes.io/docs/tasks/tools/install-minikube/)

# MANUAL INSTALLATION

## In the 3 nodes

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

### Install k8s

```bash
sudo apt-get update
sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu kubelet=1.13.5-00 kubeadm=1.13.5-00 kubectl=1.13.5-00
sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```

### Iptables

```bash
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## MASTER

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Install flannel in master

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml # < a 1.16 k8s
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/3f7d3e6c24f641e7ff557ebcea1136fdf4b1b6a1/Documentation/kube-flannel.yml # > a 1.16 k8s
```

## NODES

```bash
sudo kubeadm join $controller_private_ip:6443 --token $token --discovery-token-ca-cert-hash $hash
```

## COMMANDS

```bash
kubectl get nodes
kubectl get pod
kubectl get deployment
kubectl apply -f manifest.yaml
kubectl delete -f manifest.yaml
kubectl describe po
kubectl log
```

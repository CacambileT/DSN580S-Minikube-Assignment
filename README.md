# DSN580S T2 Assignment - Minikube on AWS

**Student Number:** 221350977
**Instance ID:** i-06b96aeb4f80ebc04

## Task 1: Provision and Secure the Instance
- Launched `t3.medium` Ubuntu instance named `minikube-221350977`.
- Created Security Group `sg-221350977-minikube` allowing SSH (port 22) from My IP only.
- Connected via SSH: `ssh -i DS.pem ubuntu@3.90.252.33`
- Captured proof of login: `echo "221350977"`, `Tue Oct 20 17:41:16 UTC 2026`, `221350977`

## Task 2: Install Docker, kubectl, and Minikube
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
rm kubectl

# Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
rm minikube-linux-amd64

# Start cluster
minikube start --driver=docker --cpus=2 --memory=3072
kubectl get nodes

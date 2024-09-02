# DevOps Toolbox

This repository contains installation instructions for various DevOps tools and services commonly used in modern software development and infrastructure management.

## Table of Contents

1. [Nginx](#nginx)
2. [Jenkins](#jenkins)
3. [Cloudflare](#cloudflare)
4. [Webmin](#webmin)
5. [MySQL](#mysql)
6. [Prometheus and Grafana](#prometheus-and-grafana)
7. [Docker](#docker)
8. [Kubernetes (K8s), Kind, and Minikube](#kubernetes-k8s-kind-and-minikube)

## Installation Instructions

### Nginx

```bash
sudo apt install nginx
```

### Jenkins

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### Cloudflare

```bash
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb
sudo cloudflared service install eyJhIjoiMzI3ZTEzYzMxNzQ2MmQ0MGVjZmM5NWE0ZDI2YTBjMTQiLCJ0IjoiNzY4ZGE4ZmEtZTJhOC00ODhiLWE2MzctMzljZjdlOTQwMzM4IiwicyI6IllUazRObVprWkdJdFpqQXpNUzAwWTJGakxXRTFOekl0WW1NeE9EVTRPV1ZqWldWaSJ9
```

### Webmin

```bash
curl -o setup-repos.sh https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh
sudo sh setup-repos.sh
sudo apt-get install webmin --install-recommends
```

### MySQL

```bash
sudo apt update
sudo apt install mysql-server
```

### Prometheus and Grafana

Please refer to the `basic-secure` repository for installation instructions.

### Docker

```bash
# Remove old versions
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do
    sudo apt-get remove $pkg
done

# Add Docker's official GPG key
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add user to docker group
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

# Verify installation
docker ps
```

### Kubernetes (K8s), Kind, and Minikube

#### Kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256) kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl

# Append ~/.local/bin to $PATH in your shell configuration file
echo 'export PATH=$PATH:~/.local/bin' >> ~/.bashrc
source ~/.bashrc
```

#### Kind

```bash
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.24.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

#### Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
rm minikube-linux-amd64
minikube start
```

## Contributing

Feel free to contribute to this repository by submitting pull requests or opening issues for any improvements or additions.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

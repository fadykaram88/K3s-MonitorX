# Senior DevOps Project

## Project Goals
1. Practical application of what I've learned in the DEBI scholarship.
2. Complete automation of all tools, avoiding manual interventions even if possible.

![Project Image](https://raw.githubusercontent.com/fadykaram88/Senior-1-/refs/heads/main/0_2AQG1Fcu8GgfKtYf.webp?token=GHSAT0AAAAAADBK4I3VNA5PDR3IJFJKK5M2Z7ILHPA)

## Tools Used
- Ansible
- Kubernetes (K3s)
- Docker
- Prometheus
- Grafana


## Infrastructure Setup

### Step 1: Install Ubuntu 22.04 LTS
Install Ubuntu 22.04 LTS on both machines from [this link](https://releases.ubuntu.com/jammy/).

### Step 2: Install Docker
```bash
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo systemctl status docker
```

### Step 3: Install K3s (Lightweight Kubernetes)
#### On Master Node:
```bash
sudo swapoff -a
sudo sed -i '/swap/d' /etc/fstab
curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s
sudo cat /var/lib/rancher/k3s/server/node-token
# Example Token: K104b4edefc218a1ab89e8400a4d3ee77a8df71b08900d9ea735113627864c93660::server:deb2864502725b2111809c8f4ba8a6b5
```
#### On Worker Node:
```bash
curl -sfL https://get.k3s.io | K3S_URL="https://<MASTER_IP>:6443" K3S_TOKEN="<NODE_TOKEN>" sh -
```
Check the nodes on the master:
```bash
kubectl get nodes
```

### Step 4: Install Prometheus and Grafana on Master Node

#### Prometheus:
```bash
curl -LO https://github.com/prometheus/prometheus/releases/latest/download/prometheus-$(uname -s)-$(uname -m).tar.gz
tar -xvf prometheus-*.tar.gz
cd prometheus-*
./prometheus --config.file=prometheus.yml
```
Create systemd service for Prometheus:
```bash
sudo nano /etc/systemd/system/prometheus.service
```
Add the following content:
```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
ExecStart=/path/to/prometheus --config.file=/path/to/prometheus.yml

[Install]
WantedBy=default.target
```
Enable and start the service:
```bash
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
curl http://localhost:9090/-/healthy
```

#### Grafana:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt update && sudo apt install -y grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
sudo systemctl status grafana-server
```
Access Grafana at `http://localhost:3000` with:
- Username: `admin`
- Password: `admin`

Link Prometheus to Grafana:
1. Open Grafana
2. Go to Configuration -> Data Source
3. Add Data Source -> Prometheus
4. URL: `http://localhost:9090`
5. Save & Test

### Step 5: Setup SSH for Ansible
On the worker node:
```bash
sudo apt update && sudo apt install -y openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```
On the master node:
```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id username@192.168.1.100
ssh username@192.168.1.100
```

### Step 6: Install Ansible
```bash
sudo apt update && sudo apt install -y software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible
ansible --version
```

## Ansible Playbooks
Create playbooks to automate the installation of Docker, Git, Prometheus, and Grafana on the worker node.


## Kubernetes Deployments

### Step 1: Deploy Nginx
```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
kubectl get services
```

### Step 2: Deploy Prometheus and Grafana
Create the following Kubernetes manifests and apply them:
- prometheus-config.yaml
- prometheus-deployment.yaml
- grafana-deployment.yaml

```bash
kubectl apply -f prometheus-config.yaml
kubectl apply -f prometheus-deployment.yaml
kubectl apply -f grafana-deployment.yaml
```

## Final Steps
1. Deploy Prometheus and Grafana in the cluster.
2. Link Prometheus with the app.
3. Link Grafana with Prometheus.
4. Add permissions for Prometheus to monitor the app.

## Notes
- The API server, scheduler, controller manager, etcd, kube-proxy, and kubelet are all created automatically after installing K3s.
- Ensure all services are running correctly and monitor them using Prometheus and Grafana.

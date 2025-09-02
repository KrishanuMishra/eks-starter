# EKS and Related Configurations

## 1. Install Docker

For **Linux (Ubuntu/Debian)**:

```bash
# Update package index
sudo apt update

# Install Docker
sudo apt install -y docker.io

# Enable and start Docker
sudo systemctl enable docker
sudo systemctl start docker

# Allow current user to run Docker without sudo (requires re-login)
sudo usermod -aG docker $USER
```

> ⚠️ Note: If you need socket-level access, use the correct path:

```bash
sudo chmod 666 /var/run/docker.sock
```

For **Mac**:  
Install Docker Desktop from [Docker’s official site](https://www.docker.com/products/docker-desktop/).

---

## 2. Install `eksctl`

Create a script `install_eksctl.sh`:

```bash
#!/bin/bash
set -e

# Set architecture (for ARM, use arm64, armv6, or armv7)
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

# Download eksctl binary
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"

# (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check

# Extract and install
tar -xzf eksctl_${PLATFORM}.tar.gz -C /tmp && rm eksctl_${PLATFORM}.tar.gz
sudo install -m 0755 /tmp/eksctl /usr/local/bin && rm /tmp/eksctl

echo "✅ eksctl installation complete!"
```

Make it executable:

```bash
chmod +x install_eksctl.sh
```

Run it:

```bash
./install_eksctl.sh
```

## 3. Managing an `EKS Cluster`:

Creating the cluster:

```bash
eksctl create cluster \
  --name eks-cluster \
  --version 1.30 \
  --region ap-south-1 \
  --nodegroup-name ng-1 \
  --node-type t3.small \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 3 \
  --managed
```

Deleting the cluster:

```bash
eksctl delete cluster --name eks-cluster --region ap-south-1
```

# Docker Installation Guide

## Remove Old Versions (if any)
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## Set Up Docker's Official Repository
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Install Docker
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Start & Enable on Boot
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## (Optional but Recommended) Run Docker Without `sudo`
```bash
sudo usermod -aG docker $USER
newgrp docker
```

## Verify Installation
```bash
docker --version
docker compose version
```

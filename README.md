# orb_install

Ubuntu 22.0.4 Ubuntu minimal installation

4 CPU , 4GB memory, 80 DRIVE
```
sudo apt update && apt upgrade
```
```
sudo timedatectl set-timezone America/Chicago
```
```
sudo apt install net-tools plocate vim git sendmail vim
```

### Docker

Add Docker's official GPG key:

```
sudo apt install ca-certificates curl
```
```
sudo install -m 0755 -d /etc/apt/keyrings
```
```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
```
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
```
sudo apt update
```

Add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt update
```
Install latest Version of Docker
```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### Helm
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
```
```
sudo apt install apt-transport-https --yes
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
```
```
sudo apt update
```
```
sudo apt install helm
```

### Kubectl
 This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
``` 
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
```
```
snap install kubectl --classic
```
```
kubectl version --client
```

### Kind
Make file for script
```
#!/bin/bash

# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo cp ./kind /usr/local/bin/kind
rm -rf kind
```
```
chmod 755 kind.sh
```
```
./kind.sh
```
```
echo "127.0.0.1 kubernetes.docker.internal" | sudo tee -a /etc/hosts
```

### installing the Control Plane

change directory
```
cd /opt/
```
Download
```
git clone https://github.com/orb-community/orb.git
```
change directory
```
cd orb
```
```
make kind-create-cluster
```
```
make prepare-helm
```
```
make dockers
```
```
make kind-load-images
```
```
make kind-install-orb
```
Takes about 5 minutes to  access Web UI.

NOTE: To correct install issues.

```
kubectl rollout restart deployment -n orb
```

### ISSUES

ERROR image orb community/orb-ui:developer not present locally.

FIX
```
docker pull orbcommunity/orb-ui:develop
```
```
kind load docker-image orbcommunity/orb-ui:develop
```


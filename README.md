# Kubernets_cluster_setup

sudo swapoff -a

sudo apt-get update

sudo -i

sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

modprobe br_netfilter

sysctl -p

# Also comment out the reference to swap in /etc/fstab. Start by editing the below file:

sudo vim  /etc/fstab

sudo reboot

# Install using the repository
# Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

# Set up the repository
# Update the apt package index and install packages to allow apt to use a repository over HTTPS:

 sudo apt-get update
 
  sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
# Add Docker’s official GPG key:

 sudo mkdir -p /etc/apt/keyrings
 
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
 
# Use the following command to set up the repository:

 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
# Install Docker Engine
# Update the apt package index, and install the latest version of Docker Engine, containerd, and Docker Compose, or go to the next step to install a specific version:

 sudo apt-get update
 
 sudo apt-get install docker-ce docker-ce-cli containerd.io 
 
 # Add the docker user in group and give permission for docker.sock
 
sudo usermod -aG docker ubuntu

sudo chmod 666 /var/run/docker.sock

sudo systemctl start docker.service

sudo systemctl status docker.service

# Enable Docker service at startup

sudo systemctl enable docker.service

sudo systemctl restart docker

# kubernetes Update the apt package index and install packages needed to use the Kubernetes apt repository:

sudo apt-get update

sudo apt-get install -y apt-transport-https ca-certificates curl

# Download the Google Cloud public signing key:

sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# Add the Kubernetes apt repository:

echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

# Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:

sudo apt-get update

sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl

sudo su ubuntu

sudo kubeadm init

# [ERROR CRI]: container runtime is not running: output:

rm /etc/containerd/config.toml

systemctl restart containerd

kubeadm init


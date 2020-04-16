
  

# Kubernetes

  

Kubernetes is a container orchestration system

  

  

!['kubernetes'](./images/k8s.png)

  

  

**In this section, we are going to build a basic Kubernetes cluster.**

  

  

## Cluster Architecture

  

<p  align=center>

  

<img  src='./images/cluster-architecture.png'  alt='Cluster Architecture'/>

  

</p>

  

  

## Setup Server

  

For building the cluster we need three servers of **ubuntu distribution**.</br>

  

>note: you can use aws ec2 instance to create server.

  

**Tag:</br>**

  

-  **For Server 1**: Kube Master </br>

-  **For Server 2**: Kube Node 1 </br>

-  **For Server 3**: Kube Node 2 </br>

  

  

## Setup Docker

<p>The first step in setting up a new cluster is to install a container runtime such as Docker.</p>

<p>We will be installing Docker on our three servers in prearation for standing up a Kubernetes cluster.</p>

  

Here are the commands :

  

```

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

  

sudo add-apt-repository \

"deb [arch=amd64] https://download.docker.com/linux/ubuntu \

$(lsb_release -cs) \

stable"

  

sudo apt-get update

  

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu

  

sudo apt-mark hold docker-ce

  

```

  

You can verify that docker is working by running this command:

  

```

sudo docker version

```

  

## Setup Kubeadm, Kubelet, and Kubectl

<p>Now that Docker is installed, we are ready to install the Kubernetes components.</p>

  

**Run these commands on all three servers.**
You can work around this by using version 1.12.7-00 for kubelet, kubeadm, and kubectl.

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.12.7-00 kubeadm=1.12.7-00 kubectl=1.12.7-00

sudo apt-mark hold kubelet kubeadm kubectl
```
After installing these components, verify that Kubeadm is working by getting the version info.

```
kubeadm version
```
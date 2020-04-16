
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

>note:  you can use aws ec2 instance to create server.

**Tag:</br>**

 - **For Server 1**: Kube Master </br>
 - **For Server 2**: Kube Node 1 </br>
 - **For Server 3**: Kube Node 2 </br>

  

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
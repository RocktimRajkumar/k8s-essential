
  

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

  

**Note:**  You can use AWS EC2 instance to initiate the server.

  

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

## Bootstrapping the Cluster
Now we are ready to get a real Kubernetes cluster up and running!. We will bootstrap the cluster on the Kube master node. Then, we will join each of the two worker nodes to the cluster, forming an actual multi-node Kubernetes cluster.

Here are the commands used in this lesson:

-   On the Kube master node, initialize the cluster:
    
    ```
    sudo kubeadm init --pod-network-cidr=10.244.0.0/16
    
    ```
    
    That command may take a few minutes to complete.
-   When it is done, set up the local kubeconfig:
    
    ```
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    
    ```
    
-   Verify that the cluster is responsive and that Kubectl is working:
    
    ```
    kubectl version
    
    ```
    
    You should get  `Server Version`  as well as  `Client Version`. It should look something like this:
    
    ```
    Client Version: version.Info{Major:"1", Minor:"12", GitVersion:"v1.12.2", GitCommit:"17c77c7898218073f14c8d573582e8d2313dc740", GitTreeState:"clean", BuildDate:"2018-10-24T06:54:59Z", GoVersion:"go1.10.4", Compiler:"gc", Platform:"linux/amd64"}
    Server Version: version.Info{Major:"1", Minor:"12", GitVersion:"v1.12.2", GitCommit:"17c77c7898218073f14c8d573582e8d2313dc740", GitTreeState:"clean", BuildDate:"2018-10-24T06:43:59Z", GoVersion:"go1.10.4", Compiler:"gc", Platform:"linux/amd64"}
    
    ```
    
-   The  `kubeadm init`  command should output a  `kubeadm join`  command containing a token and hash. Copy that command and run it with  `sudo`  on both worker nodes. It should look something like this:
    
    ```
    sudo kubeadm join $some_ip:6443 --token $some_token --discovery-token-ca-cert-hash $some_hash
    
    ```
    
-   Verify that all nodes have successfully joined the cluster:
    
    ```
    kubectl get nodes
    
    ```
    
    You should see all three of your nodes listed. It should look something like this:
    
    ```
    NAME                      STATUS     ROLES    AGE     VERSION
    wboyd1c.mylabserver.com   NotReady   master   5m17s   v1.12.2
    wboyd2c.mylabserver.com   NotReady   <none>   53s     v1.12.2
    wboyd3c.mylabserver.com   NotReady   <none>   31s     v1.12.2
    
    ```
    
    **Note:**  The nodes are expected to have a STATUS of  `NotReady`  at this point.
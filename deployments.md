## Kubernetes Deployments
Pods are a great way to organize and manage containers, but what if I want to spin up and automate multiple pods?

`Deployments` are a great way to automate the management of your pods. A deployment allows you to specify a `desired state` for a set of pods. The cluster will then constantly work to maintain that desired state.

For example:

- `Scaling`: with a deployment, you can specify the number of replicas you want, and the deployment will create (or remove) pods to meet that number of replicas.
- `Rolling Updates` : With a deployment, you can change the deployment container image to a new version of the image. The deployment will gradually replace existing containers with the new version.
- `Self-Healing`: If one of the pods in the deployment is accidentally destroyed, the deployment will immediately spin up a new one to replace it.
Deployments are an important tool if you want to take full advantage of the automation capabilities provided by Kubernetes. In this lesson, we will discuss what deployments are and briefly mention some common use cases for Kubernetes deployments. We will also create a simple deployment in our cluster and explore how we can interact with it.


Here are the commands used in this lesson:

-   Create a deployment:
    
    ```
    cat <<EOF | kubectl create -f -
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment
      labels:
        app: nginx
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:1.15.4
            ports:
            - containerPort: 80
    EOF
    
    ```
    
-   Get a list of deployments:
    
    ```
    kubectl get deployments
    
    ```
    
-   Get more information about a deployment:
    
    ```
    kubectl describe deployment nginx-deployment
    
    ```
    
-   Get a list of pods:
    
    ```
    kubectl get pods
    
    ```
    
    You should see two pods created by the deployment.
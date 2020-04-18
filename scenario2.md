## Scenario 2

**Here is the scenario**

Your team manages an online storefront. They want to have a simple service in their Kubernetes cluster that is able to provide a list of products. Other pieces of the application, running as other pods in the cluster, will use this service in the future. For now, all you need to do is deploy the service's pods to the cluster and create a Kubernetes service to provide access to those pods. The team estimates that you will need four replicas of the service pod for the time being. There is already a busybox testing pod in the cluster that you can use to test your new service once it is created.

There is a public Docker image for the store-products app called  `linuxacademycontent/store-products:1.0.0`.

You will need to do the following:

-   Create a deployment for the store-products service with four replicas.
-   Create a store-products service and verify that you can access it from the busybox testing pod.



1.  Log in to the Kube master node.
2.  Create the deployment with four replicas:
    
    ```
    cat << EOF | kubectl apply -f -
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: store-products
      labels:
        app: store-products
    spec:
      replicas: 4
      selector:
        matchLabels:
          app: store-products
      template:
        metadata:
          labels:
            app: store-products
        spec:
          containers:
          - name: store-products
            image: linuxacademycontent/store-products:1.0.0
            ports:
            - containerPort: 80
    EOF
    ```
    

Create a store-products service and verify that you can access it from the busybox testing pod.

1.  Create a service for the store-products pods:
    
    ```
    cat << EOF | kubectl apply -f -
    kind: Service
    apiVersion: v1
    metadata:
      name: store-products
    spec:
      selector:
        app: store-products
      ports:
      - protocol: TCP
        port: 80
        targetPort: 80
    EOF
    ```
    
2.  Make sure the service is up in the cluster:
    
    ```
    kubectl get svc store-products
    ```
    
    The output will look something like this:
    
    ```
    NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
    store-products   ClusterIP   10.104.11.230   <none>        80/TCP    59s
    ```
    
3.  Use  `kubectl exec`  to query the store-products service from the busybox testing pod.
    
    ```
    kubectl exec busybox -- curl -s store-products
    ```
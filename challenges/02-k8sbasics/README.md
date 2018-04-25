# Kubernetes Basics (Pods, Deployments, Services, Namespaces, Health Checks)

## Expected outcome

In this lab you will deploy some basic kubernetes resources for a sample application.

## How to

1. Create a __Pod__ manifest file (`pod.yaml`) that has the following parameters
    * _name_: my-pod
    * Uses 2 Labels 
        * _zone_ = prod
        * _version_ = v1
    * _image_ = evillgenius/kuar:1
    * Exposes port 8080

    [Example](config/pod.yaml)

2. Deploy the __Pod__ manifest

    Execute the manifest file: `kubectl create -f config\pod.yaml`
    
    Expose the Pod by port forwarding: `kubectl port-forward my-pod 8080:8080` (will run in the background)

    Verify the the endpoint locally: `http://localhost:8080`

    Stop the port forwarding command.

3. Create a __Deploy__ manifest file (`deploy.yaml`) using the same parameters as above but add
    * Create 3 replicas of the app
    * Use a RollingUpdate strategy _maxUnavailable_ = 1 and _maxSurge_ = 1

    [Example](config/deploy.yaml)

4. Deploy the __Deploy__ manifest

    Execute the manifest file: `kubectl create -f config\deploy.yaml`

    Expose the Pod by port forwarding: `kubectl port-forward my-pod 8080:8080` (will run in the background)

    Verify the the endpoint locally: `http://localhost:8080`

    Stop the port forwarding command (_Control + c_)

5. Create a __Service__ manifest file (`service.yaml`) that exposes the container using a LoadBalancer

    [Example](config/service.yaml) 

6. Deploy the __Service__ manifest

    Execute the manifest file: `kubectl create -f config\service.yaml`

    > **Note:** Creating a public endpoint can take up to a few minutes.
        You can run `kubectl get service -w` until an external IP is shown. You will also be able to view the public IP from the portal.
    
    Verify the the endpoint publicly: http://<Public IP>:8080/    

7. Clean Up
    ```
    kubectl delete pods,deployments,services,namespaces --all
    ```
    **Note:** some namespaces can not be deleted, and the kubernetes service should restart automatically.


8. Create an __App__ manifest file (`myhealthyapp.yaml') that combines the __Deploy__ and __Service__ manifests and adds Health Checks

    Add:
    * The kuar app has an http /healthy path listening on port 8080 for Liveness
    * The kuar app has an http /ready path listening on port 8080 for Readiness

    [Example](config/myhealthyapp.yaml)

9. Deploy the __App__ manifest

    Execute the manifest file: `kubectl create -f config\myhealthyapp.yaml`

10. Change your context

    The myhealthyapp manifest put everything in the 'my-ns' namespace.  The current context is the default k8s namespace.  In order to see the new deployment/services/pods, the namespace context must be my-ns.

    Use the `kubectl config view` command to view the current cluster configuration.  This should look something like:
```
[vagrant@default-centos-74 hackfest]$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: REDACTED
    server: https://hackfestk8-kubernetes-hackf-ea55d6-fa0dd490.hcp.eastus.azmk8s.io:443
  name: hackfestK8sCluster
- cluster:
    certificate-authority-data: REDACTED
    server: https://hackfestk8-kubernetes-hackf-ea55d6mgmt.eastus.cloudapp.azure.com
  name: hackfestk8-kubernetes-hackf-ea55d6mgmt
contexts:
- context:
    cluster: hackfestK8sCluster
    user: clusterUser_Kubernetes-Hackfest_hackfestK8sCluster
  name: hackfestK8sCluster
- context:
    cluster: hackfestk8-kubernetes-hackf-ea55d6mgmt
    user: hackfestk8-kubernetes-hackf-ea55d6mgmt-admin
  name: hackfestk8-kubernetes-hackf-ea55d6mgmt
current-context: hackfestk8-kubernetes-hackf-ea55d6mgmt
kind: Config
preferences: {}
users:
- name: clusterUser_Kubernetes-Hackfest_hackfestK8sCluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: dac003cceb03ceb3977f83a7bf9b28f7
- name: hackfestk8-kubernetes-hackf-ea55d6mgmt-admin
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
[vagrant@default-centos-74 hackfest]$ 
```

The "current-context" needs to bee changed.  Before changing the context, a new (human readable) context must be created.  This is done, as follows, using the information that was displayed on your terminal (similar to above):

```
kubectl config set-context my-ns --namespace=my-ns \
  --cluster=hackfestk8-kubernetes-hackf-ea55d6mgmt \
  --user=hackfestk8-kubernetes-hackf-ea55d6mgmt-admin
```

With this, the namespace will show up in the `kubectl config view` similar to the following:

```
- context:
    cluster: hackfestk8-kubernetes-hackf-ea55d6mgmt
    namespace: my-ns
    user: hackfestk8-kubernetes-hackf-ea55d6mgmt-admin
  name: my-ns
```

With the new context created, simply switch to that context and then all actions occur in that context:

```
[vagrant@default-centos-74 hackfest]$ kubectl config use-context my-ns
Switched to context "my-ns".
[vagrant@default-centos-74 hackfest]$ kubectl get pods
NAME                                READY     STATUS    RESTARTS   AGE
my-healthydeploy-2191437352-8jxn6   1/1       Running   0          1h
my-healthydeploy-2191437352-k7dph   1/1       Running   0          1h
my-healthydeploy-2191437352-lcr09   1/1       Running   0          1h
[vagrant@default-centos-74 hackfest]$ 
```

Once in the right context, you can check the status of the service to get the endpoint IP address: `kubectl get service my-healthysvc`, eventually it will show an external IP.

Verify the the endpoint publicly: http://<Public IP>:8080/

Navigate to the liveness and readiness pages: http://<Public IP>:8080/-/liveness & http://<Public IP>:8080/-/readiness

Open the K8S cluster and monitor failures as you experiment with sending failures

10. Clean Up again

## Advanced areas to explore

1. Deploy a Pod that has multiple containers in a single Pod.
2. Change the image version of your deployment and cause a RollingUpdate.
3. Rollback an update

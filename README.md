- Author Nigel Wardle. Node4

 ![Docker.](media/snakes.png "Docker") 

# Audience
Developers, Solution Architects  

## Learn to run containerised applications on AKS - Step 1 of 3

### The purpose of this training course is to remove the magic associated with deploying a single containerised application to Microsoft Azure Kubernetes Service (AKS). It uses the best practice approach that all configuration is applied through yaml text files.

## Prerequisites

1. Request access rights to your Kubernetes cluster (K8s) from your trainer
2. For MS AKS, in a browser go to https://portal.azure.com/nigelwardlehotmail.onmicrosoft.com
3. Find Azure Subscriptions and select the *Visual Studio Enterprise Subscription MPN*, or the Azure Subscription that has an AKS training cluster already provisioned.  
4. From the top search bar, search under AKS to find your cluster (aks-demo).

 ![AKS highlighted on the menu bar.](media/sub.png "AKS")

5. Select your AKS cluster.

6. From the Azure Portal, activate the Azure Command Line Interface (AZ CLI) by clicking on the  **>_** icon at the top right.

 ![The cloud shell icon is highlighted on the menu bar.](media/b4-image35.png "Cloud Shell")

7. If you have no storage provisioned - click **Create Storage** and wait for it to complete and displaying a terminal prompt

## All commands can be executed by copying from browser to the AZ CLI: >_

8. Check the version of the AZ command:

```
az version
```

9. Check the version of the kubectl.exe command (pronounced - cube-control):

```
kubectl version
```

### Does it tell you whether you are logged into the server?

10. Download credentials that give you access to the AKS Cluster:

```
az account set --subscription SubscriptionID

az aks get-credentials --name aks-demo --resource-group aks-rg
```

### Where were the downloaded credential stored?

11. Check again the version of the kubectl command:

```
kubectl version
```

### Test that the downloaded credentials work ok by entering each command below

12. test that there are worker nodes (servers) that can host your containers:

```
kubectl get nodes
```

13. count how many  kubernetes PODS there are (a container runs inside a pod):

```
kubectl get pods --all-namespaces=true
```

14. count how many Kubernetes DEPLOYMENTS there are (pods are configured and managed by deployments):

```
kubectl get deployments

kubectl get deployments --namespace kube-system
```

15. count how many Kubernetes SERVICES are (pods usually receive requests from Services):

```
kubectl get svc
```

16. see all Kubernetes (K8s) objects in the current namespace (default):

```
kubectl get all -o wide
```

#### Congratulations - you now have access to an AKS cluster and are ready to start deploying a simple web app called NGINX from a DOCKER image held in DockerHub https://hub.docker.com/_/nginx

17. Navigate in browser

*https://github.com/n4demo/learn-to-run-apps-on-kubernetes/blob/main/README-step2.md*








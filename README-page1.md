# Audience
Developers, Solution Architects

# Learn to run apps on Kubernetes
The purpose of this training course is to remove the magic associated with deploying containerised applications to a MS AKS cluster.

## Prerequisites

1. Request access rights to an AKS cluster from your trainer
2. In a browser go to https://portal.azure.com
3. Find subscriptions and select the Azure sbscription that has the AKS cluster already provisioned  
4. Search under AKS to find your cluster  (aks-demo)
5. Select your cluster.

6. From the Azure Portal activate the Azure Command Line Interface (AZ CLI)

## From now on all commands will be executed from the AZ CLI:

7. Check the version of the AZ command

*az version*

8. Download credentials that give you access to the AKS Cluster

*az aks get-credentials --name aks-demo --resource-group aks-rg*

9. Check the version of the kubectl command

*kubectl version*

### test that the downloaded credentials work ok by entering each command below

10. test that there are worker nodes (servers) that can host your containers

*kubectl get nodes*

11. count how many  kubernetes PODS there are (a container runs inside a pod)

*kubectl get pods*

12. count how many Kubernetes DEPLOYMENTS there are (pods are configured and managed by deployments)

*kubectl get deploy*

13. count how many Kubernetes SERVICES are (pods usually receive requests from Services)

*kubectl get svc*

14. see everything

*kubectl get all -o wide*

## congratulations - you now have access to AKS and are ready to start deploying a container






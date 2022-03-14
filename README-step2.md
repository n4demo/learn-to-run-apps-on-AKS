# Learn to run apps on AKS - Step 2 of 3 

 ![AKS highlighted on the menu bar.](media/k8s.png "AKS")

### Kubectl Cheat sheet https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### helpful commands:
#### list files: *ls*
#### view file: *cat my-file.yaml*
#### Show K8s objects: *kubectl get all --namespace=my-namespace*

## All commands can be executed by copying to the AZ CLI: >_

 ![The cloud shell icon is highlighted on the menu bar.](media/b4-image35.png "Cloud Shell")

1. to Save us typing - let's create a linux ALIAS for the kubectl.exe command:

```
alias k=kubectl

k version
```

2. Create temp folder and set working directory
```
mkdir temp && cd temp
```

### Now we are ready to go. To deploy our NGINX app in a container to AKS using best practice we have 4 ordered tasks to do:
- Create a new isolated space in the K8s cluster to run our container. Called a NAMESPACE
- In the new namespace create a new SERVICE ACCOUNT - this will in future link access permissions that your container has to access the AKS cluster (API)
- In the new namespace create a new DEPLOYMENT - this will store information as to what image, how many copies and where the container(s) hosted in PODS will be deployed.
- In the new namespace create a new load balancer SERVICE - this will store information as to how to expose the PODS to our browser.

### We will perform each task by creating and in future editing a YAML text file. This so we can edit and apply to any K8s cluster, saved into source control such as GIT and applied through GitOps.

## - Important. Replace test with your own first name

3. Create a new yaml formatted text file (test-ns.yaml) containing a new NAMESPACE by using the kubectl command:

```
k create namespace test --dry-run=client --output yaml > test-ns.yaml
```

4. From the AZ CLI - view the content of test-ns.yaml:

```
cat test-ns.yaml
```

5. From the AZ CLI - use the yaml file to create the actual NAMESPACE in the AKS cluster:

```
k create --filename test-ns.yaml
```

6. From the AZ CLI - switch into this NAMESPACE (if you don't, you will need to include -n=test in every following command):

```
k config set-context --current --namespace=test
```

7. Create a new yaml formatted text file (test-sa.yaml) containing a new SERVICEACCOUNT in your own NAMESPACE: 

```
k create serviceaccount test-sa --namespace=test --dry-run=client --output yaml > test-sa.yaml
```

8. From the AZ CLI - view the content of test-sa.yaml:

```
cat test-sa.yaml
```

9a. From the AZ CLI, use the file to create the actual SERVICEACCOUNT in AKS:

```
k create -n=test --filename test-sa.yaml
```

9b. Create a new yaml formatted text file (test-cm.yaml) containing a new CONFIGMAP in your own NAMESPACE:
```
k create configmap test-cm --from-literal=name=test --dry-run=client -n=test --output yaml > test-cm.yaml
```

```
k create -f test-cm.yaml
```

10. Create a new yaml formatted text file (test-deploy.yaml) containing a new DEPLOYMENT (test-deploy) in your own NAMESPACE: 

```
k create deployment test-deploy --dry-run=client -n=test --replicas=1 --image=nginx --output yaml > test-deploy.yaml
```

11. From the AZ CLI - view the contents of test-deploy.yaml:

```
cat test-deploy.yaml
```

12. From the AZ CLI - use the file to create the actual DEPLOYMENT in AKS:

```
k create -f test-deploy.yaml
```

```
k describe deploy test-deploy
```

13. Create a new yaml formatted text file (test-service.yaml) containing a new SERVICE pointing to your DEPLOYMENT (test-deploy) in your own NAMESPACE:

```
k expose deployment test-deploy --name=test-loadbalancer --type=LoadBalancer --port=80 --target-port=80 --dry-run=client -n=test -o yaml > test-service.yaml
```

14. From the AZ CLI - view the contents of test-service.yaml:

```
cat test-service.yaml
```

15. From the AZ CLI - use the file to create the SERVICE (Azure load balancer) in AKS:

```
k create -f test-service.yaml
```

```
k describe service test-loadbalancer
```

16. Check to see if all the objects are ready. Repeat until you can obtain the IP address of the load balancer: 

```
k get all -o wide
```

17. Run a browser and enter the IP address as below. All being well you should see the homepage of the NGINX app 

*http://ip-address*

```
k logs  --namespace test
```

### Congratulations - you now know how to deploy a single application container to AKS using best practice YAML files!! 

## Note: We have yet to use the ServiceAccount or ConfigMap.

18. Go to step 3








    







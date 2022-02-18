
# Learn to run apps on Kubernetes - Page 2

### Kubectl Cheat sheet https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### helpful commands:
#### list files: *ls*
#### view file: *cat my-file.yaml*
#### Show K8s objects: *kubectl get all --namespace=my-namespace*

## From now on all commands will be executed from the Azure Portal AZ CLI: >_

14. to Save us typing - let's create a linux ALIAS for the kubectl.exe command:

*alias k=kubectl*

9. Check that the alias works:

*k version*

{
## Now we are ready to go. To deploy our NGINX app in a container to AKS using best practice we have 4 ordered tasks to do:
- Create a new isolated space in the K8s cluster to run our container. Called a NAMESPACE
- In the new namespace create a new SERVICE ACCOUNT - this will in future link access permissions that your container has to access the AKS cluster (API)
- In the new namespace create a new DEPLOYMENT - this will store information as to what image, how many copies and where the container(s) hosted in PODS will be deployed.
- In the new namespace create a new load balancer SERVICE - this will store information as to how to expose the PODS to our browser.
}

## We will perform each task by creating and in future editing a YAML text file. This so we can edit and apply to any K8s cluster and should be saved into source control such as GIT 

# !!!! Important that you use you own name below without any spaces

12. Create a new yaml formatted text file (brian-ns.yaml) containing a new NAMESPACE by using the kubectl command:

*k create namespace brian --dry-run=client --output yaml > brian-ns.yaml*

13. From the AZ CLI - view the content of brian-ns.yaml:

*cat brian-ns.yaml*

14. From the AZ CLI - use the yaml file to create the actual NAMESPACE in the AKS cluster:

*k create --filename brian-ns.yaml*

15. From the AZ CLI - switch into this NAMESPACE (if you don't, you will need to include -n=brian in every following command):

*k config set-context --current --namespace=brian*

15. Create a new yaml formatted text file (brian-sa.yaml) containing a new SERVICEACCOUNT in your own NAMESPACE: 

*k create serviceaccount brian-sa --namespace=brian --dry-run=client --output yaml > brian-sa.yaml*

16. From the AZ CLI - view the content of brian-sa.yaml:

*cat brian-sa.yaml*

17. From the AZ CLI, use the file to create the actual SERVICEACCOUNT in AKS:

*k create -n=brian --filename brian-sa.yaml*

18. Create a new yaml formatted text file (brian-deploy.yaml) containing a new DEPLOYMENT (brian-deploy) in your own NAMESPACE: 

*k create deployment brian-deploy --dry-run=client -n=brian -replicas=2 --image=nginx --output yaml > brian-deploy.yaml*

19. From the AZ CLI - view the contents of brian-deploy.yaml:

*cat brian-deploy.yaml*

20. From the AZ CLI - use the file to create the actual DEPLOYMENT in AKS:

*k create -f brian-deploy.yaml*

21. Create a new yaml formatted text file (brian-service.yaml) containing a new SERVICE pointing to your DEPLOYMENT (brian-deploy) in your own NAMESPACE:

*k expose deployment brian-deploy --name=brian-loadbalancer --type=LoadBalancer --port=80 --target-port=80 --dry-run=client -n=brian -o yaml > brian-service.yaml*

19. From the AZ CLI - view the contents of brian-service.yaml:

*cat brian-service.yaml*

20. From the AZ CLI - use the file to create the SERVICE (Azure load balancer) in AKS:

*k create -f brian-service.yaml*

21. Check to see if all the objects are ready. Repeat until you can obtain the IP address of the load balancer: 

*k get all*

22. Run a browser and enter the IP address as below. All being well you should see the homepage of the NGINX app 

*http://ip-address*







    








# Learn to run apps on Kubernetes - Page 2

## Cheat sheet https://kubernetes.io/docs/reference/kubectl/cheatsheet/

## From now on all commands will be executed from the AZ CLI:

14. to Save time - let's create an ALIAS for the kubectl.exe command. Setup any easy alias.

*alias k=kubectl*

9. Check the alias works

*k version*

## Linux commands
### list files *ls*
### view file *cat my-file.yaml*
### Show K8s objects *kubectl get all --namespace=my-namespace*

## Now we are ready to go. to deploy our container to AKS using best practivce we have various tasks to perfom
- Create a new isolated space in the Kubernetes cluster called a NAMESPACE
- In the new namespace create a new SERVICE ACCOUNT - this will the link access permissions that your container has to access the AKS cluster (API)
- In the new namespace create a new DEPLOYMENT - this will store information as to how, how many copies and where the container(s) inside pods will be deployed.
- In the new namespace create a new SERVICE - this will store information as to how to expose the pods to the outside world.

## We will perform the previous steps by creating YAML text files so these can eventually be deployed into source control such as GIT 

# !!!! Important that you use you own name below without any spaces

12. Create a new yaml formatted text file (ns.yaml) containing a new NAMESPACE by using the kubectl command

*k create namespace brian --dry-run=client --output yaml > brian-ns.yaml*

13. From the AZ CLI - view the content of ns.yaml

*cat brian-ns.yaml*

14. From the AZ CLI - use the yaml file to create the actual NAMESPACE in the AKS cluster

*k create --filename brian-ns.yaml*

15. From the AZ CLI - switch into this NAMESPACE

k config set-context --current --namespace=brian

15. Create a new yaml formatted text file (sa.yaml) containing a new SERVICEACCOUNT in your own NAMESPACE 

*k create serviceaccount brian-sa --namespace=brian --dry-run=client --output yaml > brian-sa.yaml*

16. From the AZ CLI - view the content of brian-sa.yaml

*cat brian-sa.yaml*

17. From the AZ CLI - use the file to create the actual SERVICEACCOUNT in AKS

*k create -n=brian --filename brian-sa.yaml*

18. Create a new yaml formatted text file (brian-deploy.yaml) containing a new DEPLOYMENT (brian-deploy) in your own NAMESPACE 

*k create deployment brian-deploy --dry-run=client -n=brian --image=nginx --output yaml > brian-deploy.yaml*

19. From the AZ CLI - view the content of brian-deploy.yaml

*cat brian-deploy.yaml*

20. From the AZ CLI - use the file to create the actual DEPLOYMENT in AKS

*k create -f brian-deploy.yaml*

21. Create a new yaml formatted text file (brian-service.yaml) containing a new SERVICE pointing to your DEPLOYMENT (brian-deploy) in your own NAMESPACE 

*k expose deployment brian-deploy --name=brian-loadbalancer --type=LoadBalancer --port=80 --target-port=80 --dry-run=client -n=brian -o yaml > brian-service.yaml*

19. From the AZ CLI - view the content of brian-deploy.yaml

*cat brian-service.yaml*

20. From the AZ CLI - use the file to create the actual DEPLOYMENT in AKS

*k create -f brian-service.yaml*

21. Finally, check to see if all the objects are ready. Repeat until you can obtain the IP address of the load balancer and enter into browser http://ip-address

*k get all*

*ls*







    







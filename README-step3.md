
# Learn to run apps on Kubernetes - Step 3

## We now need to secure the configuration of the DEPLOYMENT by updating the yaml file and re-deploying:

- Edit POD configuration to use the SERVICEACCOUNT we created earlier
- Edit DEPLOYMENT to increase the number of PODS each hosting a single NGINX container to 2 replicas (copies)
- Edit the POD RESOURCES configuration so that it REQUESTS only 20% of a CPU Core upon startup and LIMITS to 30% of CPU as a maximum
- Edit the POD RESOURCES configuration so that it REQUESTS only 200MB of memory upon startup and LIMITS to 250MB of memory as a maximum.
- Edit the POD SECURITYCONTEXT so the containers do not have root (admin) privileges
  Edit DEPLOYMENT to READINESS and LIVENESSPROBES to determine when a POD is ready and if it is still responsive


## We will perform each task by editing the DEPLOYMENT YAML text file using the AZ CLI online editor. This so we can re-apply to our K8s cluster and being saved into source control such as GIT. 

# - Important. Replace brian with you own firstname:

18. Edit Create a new yaml formatted text file (brian-deploy.yaml) containing a new DEPLOYMENT (brian-deploy) in your own NAMESPACE: 

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







    








# Learn to run apps on Kubernetes - Step 3

## We now need to secure the configuration of the DEPLOYMENT by updating the yaml file and re-deploying:

- Edit POD configuration to use the SERVICEACCOUNT we created earlier
- Edit DEPLOYMENT to increase the number of PODS each hosting a single NGINX container to 2 replicas (copies)
- Edit the POD RESOURCES configuration so that it REQUESTS only 20% of a CPU Core upon startup and LIMITS to 30% of CPU as a maximum
- Edit the POD RESOURCES configuration so that it REQUESTS only 200MB of memory upon startup and LIMITS to 250MB of memory as a maximum.
- Edit the POD SECURITYCONTEXT so the containers do not have root (admin) privileges
  Edit DEPLOYMENT to READINESS and LIVENESSPROBES to determine when a POD is ready and if it is still responsive

## We will perform each task by editing the DEPLOYMENT yaml text file using the AZ CLI online editor. This so we can re-apply to our K8s cluster and being saved into source control such as GIT.

23. Open the deployment yaml file (firstname-deploy.yaml) by first clicking the icon for the AZ CLI editor {} and then double clicking on your deployment yaml file.

24. Work your way down the file in AZ CLI adding in the new code as described below:

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: firstname-deploy
  name: firstname-deploy
  namespace: firstname
spec:
  replicas: 1
  selector:
    matchLabels:
      app: firstname-deploy
  strategy: {}
  template:
    metadata:
      labels:
        app: firstname-deploy
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}



20. From the AZ CLI - use the file to update the actual DEPLOYMENT in AKS:

*k apply -f firstname-deploy.yaml*

21. Check to see if all the objects are ready. Repeat until you can obtain the IP address of the load balancer: 

*k get all -n firstname*

22. Run a browser and enter the IP address as below. All being well you should see the homepage of the NGINX app 

*http://ip-address*







    







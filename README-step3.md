# Learn to run apps on AKS - Step 3 of 3

## Configure the DEPLOYMENT to best practice by updating the yaml file and re-deploying:

- Apply DEPLOYMENT POD SERVICEACCOUNT To not mount a security token hence revoking permissions to the K8s API server.
- Apply DEPLOYMENT POD SECURITYCONTEXT so the containers do not run as root (admin) privileges and cannot escalate.
- Apply DEPLOYMENT REPLICAS to increase the number of PODS each hosting a single NGINX container to 2.
- Apply DEPLOYMENT POD RESOURCES so it REQUESTS only 20% of a CPU Core upon startup and LIMITS to 20% of CPU.
- Apply DEPLOYMENT POD RESOURCES so it REQUESTS only 200MB of memory at startup and LIMITS to 200MB of memory.
- Apply DEPLOYMENT READINESS and LIVENESSPROBES to probe when a container is ready to receive requests and still responsive.

## Edit the DEPLOYMENT yaml text file using the AZ CLI online editor {}. 

23. Copy the code below to an editor and perform a search and replace on firstname to your name.  

24. Open the deployment yaml file (firstname-deploy.yaml) by first clicking the icon for the AZ CLI editor {} and then pasting over the new code.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: firstname-deploy
  name: firstname-deploy
  namespace: firstname
spec:
  replicas: 2
  selector:
    matchLabels:
      app: firstname-deploy
  strategy: {}
  template:
    metadata:
      labels:
        app: firstname-deploy
    spec:
      securityContext:
        readOnlyRootFilesystem: true
        runAsUser: 1000
          capabilities:
          add: ["SYS_TIME"]
        allowPrivilegeEscalation: false
      serviceAccountName: firstname-sa
      automountServiceAccountToken: false
      containers:
      - image: nginx
        name: nginx
        resources:
          limits:
            memory: "200M"
            cpu: 0.20
          requests:
            memory: "200M"
            cpu: 0.20
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 3
          periodSeconds: 3
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 3
          periodSeconds: 3    
```

25. From the Editor click ... Save

26. From the AZ CLI - list the file then use the file to update the actual DEPLOYMENT in AKS:

```
ls
```

```
k apply -f firstname-deploy.yaml
```

27. Now get K8s to return a yaml for the updated configuration:

```
k get deploy firstname-deploy -o yaml
```

28. Check to see if all the objects are ready. Repeat until you can obtain the IP address of the load balancer: 

```
k get all -n firstname
```

```
firstname@Azure:~$ k get all -n firstname
NAME                                    READY   STATUS    RESTARTS   AGE
pod/firstname-deploy-659554944c-kwkd6   1/1     Running   0          15m
pod/firstname-deploy-659554944c-qsf7g   1/1     Running   0          15m

NAME                             TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)        AGE
service/firstname-loadbalancer   LoadBalancer   10.0.148.1   20.108.129.15   80:30209/TCP   21m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/firstname-deploy   2/2     2            2           22m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/firstname-deploy-659554944c   2         2         2       15m
replicaset.apps/firstname-deploy-74bccf899c   0         0         0       22m
```

29. Run a browser and enter the IP address as below. All being well you should see the homepage of the NGINX app 

*http://20.108.129.15*

30. In the Azure Portal select the AKS cluster and click Workloads. Navigate freely to see K8s objects that have been created.

### Congratulations! Next steps: https://www.cncf.io/certification/ckad/

### Optional: 

31. Create a configmap object containing an environment variable firstname=brian. Injec this environment variable into the POD. 
#### hint https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/




    







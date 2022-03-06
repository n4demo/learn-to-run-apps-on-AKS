## Learn to run containerised applications on AKS - Quiz

1. When running Kubectl, where are the credentials stored that enable access to the Kubernetes cluster?

2. If I create a deployment yaml file and then apply it to the cluster and it creates a pod: nginx-123-344-56. What will happen if I use the AKS dashboard, select this POD and delete it?

3. I need to deploy three micro-service based apps into AKS and I want to follow best practice. After I have completed this task, how many yaml files should end up being stored in my source control repository? Name them.

4. In a non production environment I may choose to use the following commands:

```
kubectl create deployment nginx --image=nginx
kubectl create cronjob hello --image=busybox   --schedule="*/1 * * * *" -- echo "Hello World"
kubectl expose rc nginx --port=80 --target-port=8000
kubectl autoscale deployment foo --min=2 --max=10
kubectl scale --replicas=5 rc/foo rc/bar rc/baz
kubectl delete pods,services -l name=myLabel
```

5. Would I use these commands in a production environment? If not how would I edit my Kubernetes resources?





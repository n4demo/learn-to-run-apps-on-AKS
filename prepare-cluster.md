### create a new resource group in your Azure subscription  
az group create --location uksouth --name aks-rg

### create AKS cluster using your *** unique name **
az aks create --resource-group aks-rg --name aks-demo --generate-ssh-keys

### download credentials into local file
az aks get-credentials --name aks-demo --resource-group aks-rg

# test that credentials work ok
kubectl get all

### deploy the nginx web server image from Docker Hub into your kubernetes cluster
kubectl run  –n nginx --image=nginx  
kubectl expose deployment nginx --name=nginx

### now determine IP address of nginx server
kubectl get all -o wide

### access web server in browser
http://<ip-address>

### now deploy the node4demo/python-standard:v1.0.0 web server image from Docker Hub into your kubernetes cluster

node4demo/python-standard:v1.0.0

### deploy python in a pod in kubernetes
kubectl run  python --image=node4demo/python-standard:latest  

### add a load balancer service on a public IP
kubectl expose deployment python --type=LoadBalancer --name=python --port=80 --target-port=5000

### now determine public IP address of python service
kubectl get all -o wide

### access python web server in browser
http://<ip-address>

### alternatively deploy python app to your kubernetes using the supplied YAML file
### in the Azure CLI - upload python-kube-manifest.yaml
kubectl apply -f python-kube-manifest.yaml

kubectl get all -o wide




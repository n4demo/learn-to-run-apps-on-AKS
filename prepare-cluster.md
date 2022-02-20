
1. create a new resource group in your Azure subscription 

* az group create --location uksouth --name aks-rg 

2. create AKS cluster using your *** unique name **

*az aks create --resource-group aks-rg --name aks-demo --generate-ssh-keys*

3. download credentials into local file

*az aks get-credentials --name aks-demo --resource-group aks-rg*

4. test that credentials work ok

*kubectl get all*

5. deploy the nginx web server image from Docker Hub into your kubernetes cluster

*kubectl run  –n nginx --image=nginx * 

*kubectl expose deployment nginx --name=nginx*

6. now determine IP address of nginx server

*kubectl get all -o wide*

7. access web server in browser

*http://<ip-address>*

8. Deploy the node4demo/python-standard:v1.0.0 web server image from Docker Hub into your kubernetes cluster

* node4demo/python-standard:v1.0.0*

9. deploy python in a pod in kubernetes

*kubectl run  python --image=node4demo/python-standard:latest  *

10. add a load balancer service on a public IP

*kubectl expose deployment python --type=LoadBalancer --name=python --port=80 --target-port=5000*

11. now determine public IP address of python service

*kubectl get all -o wide*

12. access python web server in browser

*http://ip-address*

13. alternatively deploy python app to your kubernetes using the supplied YAML file

14. in the Azure CLI - upload python-kube-manifest.yaml

*kubectl apply -f python-kube-manifest.yaml*

*kubectl get all -o wide*




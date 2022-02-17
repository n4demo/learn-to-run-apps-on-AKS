

## From now on all commands will be executed from the AZ CLI:

14. to Save time - let's create an ALIAS for the kubectl.exe command. Setup any easy alias.

*alias k=kubectl*

9. Check the alias works

*k version*

### Now we are ready to go. to deploy our container to AKS using best practivce we have various tasks to perfom
- Create a new isolated space in the Kubernetes cluster called a NAMESPACE
- In the new namespace create a new SERVICE ACCOUNT - this will store access permissions to the AKS cluster (API)
- In the new namespace create a new DEPLOYMENT - this will store information as to how, how many copies and where the container(s) inside pods will be deployed.
- In the new namespace create a new SERVICE - this will store information as to how to expose the pods to the outside world.

test that the downloaded credentials work ok by entering each command below

10. Create the new namespace directly using the kubectl command - use you own name without any spaces

*k create namespace my-name*








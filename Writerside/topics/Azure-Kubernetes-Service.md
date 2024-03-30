# Azure Kubernetes Service

Azure Kubernetes Service (AKS) is a managed Kubernetes service provided by Microsoft Azure. It simplifies the deployment, management, and scaling of containerized applications using Kubernetes. AKS offers a fully managed Kubernetes cluster that can be easily integrated with other Azure services.

## Azure CLI

After installing the Azure CLI, I attempted to deploy some containers to AKS. However, after running the command `az aks get-credentials --resource-group myResourceGroup --name myAKSCluster`, I received the following error:

```Bash
The client '[ACCOUNT-EMAIL]' with object id '[OBJECT-ID]' does not have authorization to perform action 'Microsoft.ContainerService/managedClusters/listClusterUserCredential/action' over scope '[CLUSTER-NAME]' or the scope is invalid. If access was recently granted, please refresh your credentials.
```

After some messing around, I found the solution to be setting the subscription to the Student subscription. First I listed the subscriptions with:

```Bash
az account list
```

Then I set the subscription to the Student subscription with:

```Bash
az account set --subscription [STUDENT-SUBSCRIPTION-ID]
```

After setting the subscription, I was able to successfully run the `az aks get-credentials --resource-group myResourceGroup --name myAKSCluster` command and access the AKS cluster.
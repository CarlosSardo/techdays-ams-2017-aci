
# TechDays 2017

## Create your first container in Azure Container Instances (Preview)

Azure Container Instances makes it easy to create and manage Docker containers in Azure, without having to provision virtual machines or adopt a higher-level service. In this quick start, you create a container in Azure and expose it to the internet with a public IP address. This operation is completed in a single command. Within just a few seconds, you'll see this in your browser:

If you choose to install and use the CLI locally, this quick start requires that you are running the Azure CLI version 2.0.12 or later. Run `az --version` to find the version. If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).

## Create a resource group

Azure Container Instances are Azure resources and must be placed in an Azure resource group, a logical collection into which Azure resources are deployed and managed.

Create a resource group with the [az group create][az-group-create] command.

The following example creates a resource group named *TechDays2017* in the *westeu* location.

```azurecli-interactive
az group create --name techdays2017resourcegroup --location westeu
```

## Create a container

You can create a container by providing a name, a Docker image, and an Azure resource group to the [az container create][az-container-create] command. You can optionally expose the container to the internet with a public IP address. In this case, we'll use a container that hosts a very simple web app written in [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name techdays2017container --image microsoft/aci-helloworld --resource-group techdays2017resourcegroup --ip-address public
```

Within a few seconds, you should get a response to your request. Initially, the container will be in a **Creating** state, but it should start within a few seconds. You can check the status using the [az container show][az-container-show] command:

```azurecli-interactive
az container show --name techdays2017container --resource-group techdays2017resourcegroup
```

At the bottom of the output, you will see the container's provisioning state and its IP address:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Once the container moves to the **Succeeded** state, you can reach it in your browser using the IP address provided.

## Pull the container logs

You can pull the logs for the container you created using the [az container logs][az-container-logs] command:

```azurecli-interactive
az container logs --name techdays2017container --resource-group techdays2017resourcegroup
```

Output:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## Delete the container

When you are done with the container, you can remove it using the [az container delete][az-container-delete] command:

```azurecli-interactive
az container delete --name techdays2017container --resource-group techdays2017resourcegroup
```

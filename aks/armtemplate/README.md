**Azure Resource Manager Template for the creation of Azure Container Service (AKS) cluster**

- [Steps](#steps)
    - [Create SSH Key](#create-ssh-key)
    - [Authenticate azure CLI & Select subscription](#authenticate-azure-cli-select-subscription)
    - [Create a Resource Group](#create-a-resource-group)
    - [Create Service Principal and assign role](#create-service-principal-and-assign-role)
    - [Update the parameters in the Azure Resource Manager Template parameter file](#update-the-parameters-in-the-azure-resource-manager-template-parameter-file)
    - [Deploy the cluster using the Azure Resource Manager Template](#deploy-the-cluster-using-the-azure-resource-manager-template)
    - [Connect to the cluster](#connect-to-the-cluster)
    - [Steps to create Key Vault and set secret](#steps-to-create-key-vault-and-set-secret)

# Steps

## Create SSH Key

Generate a SSH key pair to use in the creation of the AKS cluster. You can find a detailed instructions on how to create an SSH key pair at the URL below. 

https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

## Authenticate azure CLI & Select subscription
        
        az login
        az account set --subscription "subscriptionID"

## Create a Resource Group 
**Make sure you choose a location where AKS is available**

        az group create --name <resourceGroupName> --location <region>

 ## Create Service Principal and assign role
        
        az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>"

 ## Update the parameters in the Azure Resource Manager Template parameter file
        
        Update SSH public key, Service Principal credentails. If you are using the key vault to store the secrets, use the key vault reference in the parameter file. Steps to create the key vault is given below
    
                "servicePrincipalClientSecret": {
                    "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subscriptionid>/resourceGroups/<keyVaultResourceGroup>/providers/Microsoft.KeyVault/vaults/<keyVaultName>"
                    },
                    "secretName": "key vault secret name"
                    }
                }

 

 ## Deploy the cluster using the Azure Resource Manager Template
       
        az group deployment create --resource-group <resourceGroupName> --template-file k8smanaged-azuredeploy.json --parameters @k8smanaged-azuredeploy.parameters.json


## Connect to the cluster
        
        az aks get-credentials  --name <clusterName> --resource-group <resourceGroupName>




## Steps to create Key Vault and set secret
    
    Create Key Vault and enable for template deployment
    
        az keyvault create --name <keyVaultName> --resource-group <keyVaultresourceGroupName> --enabled-for-template-deployment

    Set the policy so that the service principal can access the key vault
        
        az keyvault set-policy --name <keyVaultName> --spn <servicePrincipalId> --secret-permissions get 

    Set the service principal password as secret
        
        az keyvault secret set --name <keyVaultSecretName> --vault-name <keyVaultName> --value <servicePrincipalPassword>




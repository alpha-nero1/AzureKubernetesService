# Starting with AKS

Download azure cli: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-macos

az aks reference: https://learn.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest

An AKS cluster can be created either via the az cli or the azure portal.

## Create a resource group
`az group create -l eastus -n aks-rg`

## Create a cluster
`az aks create -g aks-rg -n aks --node-count 2 --generate-ssh-keys`

Generate ssh keys generates private and public key files if missing.

## We can then easily install kubectl with
`az aks install-cli`

`kubectl version`

You can connect to your cluster via kubectl or azure cloud shell.
the azure portal provides instructions on how to do this.
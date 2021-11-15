## Setup

Set environment defaults.

```sh
SUBSCRIPTION_ID='5c8f3e12-757e-4183-a8d0-80effb7bf97f'
RESOURCE_GROUP='saty-aks-rg'
LOCATION=eastus2
CLUSTER_NAME='saty-aks'
```

Create an AKS cluster with CNI, managed identity, pod identity add-on enabled and uses ephemeral OS disc. 

```sh
az group create \
  --name $RESOURCE_GROUP \
  --location $LOCATION

az aks create \
  --resource-group $RESOURCE_GROUP \
  --name $CLUSTER_NAME \
  --enable-managed-identity \
  --enable-pod-identity \
  --node-osdisk-type Ephemeral \
  --network-plugin azure
```


## Deploy

Use `kubectl` to deploy the `KEDA` manifest files.

```sh
kubectl apply -f https://github.com/kedacore/keda/releases/download/v2.2.0/keda-2.2.0.yaml
```

Verify the installation of `KEDA`.

```sh
kubectl get pods --namespace keda
```

At this point `KEDA` is up and running; however, no workload is currently under `KEDA` scaling control. `KEDA` still needs to be configured to monitor your workload for scaling. This will be done in a future step.

## Troubleshooting

## Cleanup
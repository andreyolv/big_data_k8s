```shell
#  microsoft azure
# https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
az account set --subscription 495322cb-95ae-4e66-b31d-1ea25d0b4ada
az aks get-credentials --resource-group k8s-aks-owshq-dev --name aks-owshq-dev

# google gcp
# https://cloud.google.com/sdk/gcloud
gcloud container clusters get-credentials silver-charmer-243611-gke --region us-central1

# digital ocean
# https://docs.digitalocean.com/reference/doctl/how-to/install/
doctl kubernetes cluster kubeconfig save 69aa8706-603d-46a4-8804-def3733675c0

# linode
# https://www.linode.com/docs/guides/linode-cli/
cluster_id=44470
linode-cli lke clusters-list
linode-cli lke kubeconfig-view $cluster_id

# local kube config
kubectl config view
$HOME/.kube/config
```

```shell
# microsoft azure aks cost
# 7 = virtual machines (ds3v2)
# 4 vcpus & 14 gb of ram
# 1,170.19
# ~ R$ 10.000
```
https://docs.digitalocean.com/reference/doctl/how-to/install/
https://docs.digitalocean.com/reference/
https://registry.terraform.io/providers/digitalocean/digitalocean/latest/docs

```shell
# digital ocean

# install doctl
brew install doctl
brew upgrade doctl
doctl --help

# api token for access
https://cloud.digitalocean.com/account/api/tokens?i=41b6a9
https://docs.digitalocean.com/reference/api/create-personal-access-token/

# authentication process
# token = 348fd66dc6a04b53a9aa2884f2b2e7df5df2477ec8cacb2f4e726b11d7cae085
doctl auth list
doctl auth init

# kubernetes info
doctl kubernetes cluster 
doctl kubernetes options sizes 
doctl kubernetes cluster list
doctl kubernetes cluster node-pool list 7bddf3b0-1139-4006-a7e6-56a16caef77b

# get cluster context 
doctl kubernetes cluster kubeconfig save 7bddf3b0-1139-4006-a7e6-56a16caef77b
```

```shell
# access iac terraform script
/Users/luanmorenomaciel/BitBucket/big-data-on-k8s/iac/do/do-nyc1-do-owshq-dev/

# init terraform script process
# prepare working directory
terraform init

# build plan to build 
# changes required
terraform plan

# apply creation iac code
# create resources
terraform apply -auto-approve

# access cluster
# kubernetes aks engine
doctl kubernetes cluster list
doctl kubernetes cluster node-pool list []

# change [variables.tf]
terraform plan
terraform apply

# remove resources [rg]
# destroy resources
terraform destroy -auto-approve

```
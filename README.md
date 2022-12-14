# big_data_k8s

## k3d Kubernetes Cluster
```sh 
k3d cluster create bigdatak8s --volume $HOME/bigdatak8s:/var/lib/rancher/k3s/storage@all -s 1 --servers-memory 4Gb -a 3 --agents-memory 12Gb --api-port 6443 -p 8081:80@loadbalancer
```
  - storage class: local-path em $HOME/bigdatak8s
  - 1 control plane - 4Gb
  - 3 worker node - 12Gb
  - port-foward 80 para 8081

## Argo CD
- Helm chart do ArgoCD baixado e yaml values alterado para usar ingress (params.server.insecure: true,params.server.rootpath: '/argocd')
- Atualiza o ArgoCD com yaml values alterado:
```sh 
helm upgrade --install -f https://raw.githubusercontent.com/andreyolv/big_data_k8s/main/repository/helm-charts/argo-cd/values.yaml argocd argo/argo-cd --namespace cicd --debug --timeout 10m0s
```
- Aplica o yaml abaixo para usar ingress:
```sh 
k apply -f repository/svc-lbs/svc_ingress_argocd.yaml
```
- Olhar cluster:
```sh 
kubectl get all -n cicd
```
- App of Apps: 
```sh 
kubectl apply -f https://raw.githubusercontent.com/andreyolv/big_data_k8s/main/bigdatak8s.yaml
```
- Loga no ArgoCD: [http://127.0.0.1:8081/argocd/login](http://127.0.0.1:8081/argocd/login)
  - user: admin
  - password: 
  ```sh 
  kubectl -n cicd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d  | more
  ```
- Caso algo fique como degraded basta deletar, o argo irá recriar novamente. Apenas não delete o cicd/k8sexample

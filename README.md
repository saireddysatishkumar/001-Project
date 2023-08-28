# 001-Project
ToDoApp-python-jenkins-argocd-k8s

## Prerequistes
>
- Create Docker Hub account and API Token.
- Create Git repos - 2. One is for CI(ex: current repo) and another one is for CD (ex:[ArgoCD](https://github.com/saireddysatishkumar/ArgoCD)).  Copy and commit these 2 repo's code to your respective repos.
- Jenkins (Added label python Jenkins node's and create a pipeline pointing CI repo(current)).  
- EKS or AKS or [Minikube Cluster](https://github.com/saireddysatishkumar/K8S/tree/main/Minikube).  
- Helm installed on your local machine.
- Configure kubectl to access your cluster.
- [Install ArgoCD](https://github.com/saireddysatishkumar/ArgoCD)



## Use Case 1: 
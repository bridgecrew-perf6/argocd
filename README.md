# argocd
Manage deployments

ArgoCD, Kubernetes (Here with minikube), here used for Kafka and Strimzi

## Install
```bash
minikube start # and more params to set more mem and cpu, k8s version
kubectl create ns argocd
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kubectl create ns argocd
helm install -n argocd local-argocd argo/argo-cd
k9s
kubectl port-forward service/local-argocd-server -n argocd 8080:443
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
brew install argocd # Mac user only otherwise visit https://github.com/argoproj/argo-cd/releases/tag/v2.2.2
argocd login localhost:8080 # Give credential same using the UI
argocd repo add https://github.com/dfollereau/argocd.git # Connect a repo
argocd repo list
kubectl apply -f argocd/projects/project-dev.yml
argocd proj list
kubectl create ns strimzi
kubectl create ns kafka
kubectl apply -f argocd/root-app-dev.yaml
argocd app sync root-appbundle-app-dev
argocd app list
argocd app sync helm-strimzi-setup-dev
argocd app sync helm-strimzi-deploy-dev
argocd app sync helm-kafka-deploy-dev
```
## License
[MIT](https://choosealicense.com/licenses/mit/)

# argocd
Manage deployments

ArgoCD, Kubernetes (Here with minikube), here used for Kafka and Strimzi

## Install
```bash
minikube start # and more params to set more mem and cpu, k8s version
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
kubectl apply -f argo/projects/project-dev.yml
argocd proj list
kubectl create ns strimzi
kubectl create ns kafka
kubectl apply -f argo/root-app-dev.yaml
argocd app sync root-appbundle-app-dev
argocd app list
argocd app sync helm-strimzi-setup-dev
argocd app sync helm-strimzi-deploy-dev
argocd app sync helm-kafka-deploy-dev
```

## Install 2 (alternative)
```bash
minikube start # and more params to set more mem and cpu, k8s version
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
minikube tunnel --cleanup=true
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
brew install argocd # Mac user only otherwise visit https://github.com/argoproj/argo-cd/releases/tag/v2.2.2
argocd login localhost
argocd version
argocd repo add https://github.com/dfollereau/argocd.git # Connect a repo
argocd repo list
kubectl apply -f argo/projects/project-dev.yml
argocd proj list
kubectl create ns strimzi
kubectl create ns kafka
kubectl apply -f argo/root-app-dev.yaml
argocd app sync root-appbundle-app-dev
argocd app list
argocd app sync helm-strimzi-setup-dev
argocd app sync helm-strimzi-deploy-dev
argocd app sync helm-kafka-deploy-dev
```

## Uninstall
```bash
helm list --all-namespaces
helm delete -n argocd local-argocd
#OR (if Install 2) kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
minikube stop
minikube delete
```

## License
[MIT](https://choosealicense.com/licenses/mit/)

GitOps-Argocd
GitOps with ArgoCD on Kubernetes

This project demonstrates GitOps using ArgoCD and Minikube. Kubernetes deployments are automatically synced from a GitHub repository.

Tools Used
Kubernetes (Minikube)
ArgoCD
GitHub
Docker
How It Works
1.  Kubernetes manifest files (deployment.yaml, service.yaml) are stored in a GitHub repo.
2.  ArgoCD watches the repo and syncs changes to the cluster.
3.  When a change is pushed to Git (e.g., updating image version), ArgoCD applies it automatically.

Steps to Run

1. Start Minikube:

minikube start --driver=docker Install ArgoCD:

kubectl create namespace argocd kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml Port forward the ArgoCD UI:

kubectl port-forward svc/argocd-server -n argocd 8080:443 Get the ArgoCD admin password:

kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d Deploy app using ArgoCD CLI:

argocd login localhost:8080 --username admin --password --insecure argocd app create nginx-app
--repo(https://github.com/phanindra4568/gitops-app.git)//gitops-demo.git
--path k8s
--dest-server https://kubernetes.default.svc
--dest-namespace default
--sync-policy automated Demo Change image in deployment.yaml in GitHub (e.g., nginx:1.25 → nginx:1.26)

Commit and push

ArgoCD auto-syncs and updates the deployment in Kubernetes

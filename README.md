GitOps-Argocd
GitOps with ArgoCD on Kubernetes

This project demonstrates GitOps using ArgoCD and Minikube. Kubernetes deployments are automatically synced from a GitHub repository.

Tools Used:
Kubernetes (Minikube)

ArgoCD

GitHub

Docker

How It Works:
Kubernetes manifest files (deployment.yaml, service.yaml) are stored in a GitHub repository.

ArgoCD watches the repository and syncs changes to the Kubernetes cluster.

When a change is pushed to Git (e.g., updating the image version), ArgoCD automatically applies the change to the cluster.

Steps to Run:
1. Start Minikube:

minikube start --driver=docker
2. Install ArgoCD:

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
3. Port forward the ArgoCD UI:

kubectl port-forward svc/argocd-server -n argocd 8080:443
4. Get the ArgoCD admin password:

kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
5. Deploy the app using the ArgoCD CLI:

argocd login localhost:8080 --username admin --password <your_password> --insecure
argocd app create nginx-app --repo https://github.com/mrunalini12/gitops-by-mrunalini.git --path k8s --dest-server https://kubernetes.default.svc --dest-namespace default --sync-policy automated
6. Demo:
Change the image version in deployment.yaml in GitHub (e.g., nginx:1.25 → nginx:1.26).

7. Commit and push changes:


git commit -m "Update nginx version"
git push
ArgoCD will automatically sync and update the deployment in Kubernetes.

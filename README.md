# jenkins-argocd-gitops

Source-of-truth repo for ArgoCD. This is what ArgoCD watches and syncs
the cluster against - **never `kubectl apply` these manually once ArgoCD
is managing them** (that defeats the point, and you'll get drift/fight
between manual changes and ArgoCD's self-heal, which is actually a good
Pillar 4 troubleshooting exercise to try once on purpose).

## Structure
- `manifests/deployment.yaml` - the app Deployment (image tag gets bumped
  here by Jenkins)
- `manifests/service.yaml` - NodePort Service exposing the app
- `manifests/ecr-pull-secret-template.yaml` - reference for ECR auth (not
  auto-applied - ECR tokens expire every 12hrs, this is a deliberate
  troubleshooting scenario)
- `argocd-apps/application.yaml` - the ArgoCD Application CRD itself,
  applied directly on the cluster to tell ArgoCD to start watching this repo

## Before first use
1. Replace `your-username` in `argocd-apps/application.yaml` with your
   actual GitHub username
2. Replace the image in `manifests/deployment.yaml` with your actual
   Docker Hub username once you've created that repo
3. Apply the Application CRD on EC2-2 after ArgoCD is installed:
   ```
   kubectl apply -f argocd-apps/application.yaml
   ```
test

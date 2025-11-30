# Tintin Argo CD Applications

This repository contains sample Argo CD application manifests for managing Kubernetes deployments using Argo CD.

## Repository Structure

```
├── apps/                           # Individual application definitions
│   ├── nginx-app/                  # Nginx deployment example
│   │   ├── application.yaml        # Argo CD Application resource
│   │   └── deployment.yaml         # Kubernetes manifests
│   ├── sample-app/                 # Sample application example
│   │   ├── application.yaml        # Argo CD Application resource
│   │   └── deployment.yaml         # Kubernetes manifests
│   └── ...
└── README.md                       # This file
```

## Applications Included

### 1. Nginx App
- **Path**: `apps/nginx-app/`
- **Description**: Nginx webserver deployment with 2 replicas
- **Namespace**: default
- **Replicas**: 2

### 2. Sample App
- **Path**: `apps/sample-app/`
- **Description**: Sample application deployment with dedicated namespace
- **Namespace**: sample-app
- **Replicas**: 1

## Prerequisites

- Argo CD installed and running on your Kubernetes cluster
- kubectl configured to access your cluster
- GitHub token with repository access (if repository is private)

## Quick Start

### 1. Add Repository to Argo CD

```bash
argocd repo add https://github.com/TamilTintin/tintin-argocd-applications \
  --username TamilTintin \
  --password <your-github-token>
```

### 2. Create Applications

Apply individual application manifests:

```bash
# Deploy Nginx app
kubectl apply -f apps/nginx-app/application.yaml

# Deploy Sample app
kubectl apply -f apps/sample-app/application.yaml
```

### 3. Verify Applications

```bash
# List Argo CD applications
argocd app list

# Check application status
argocd app get nginx-app
argocd app get sample-app
```

## Sync Policies

All applications use the following sync policies:
- **Automated**: Automatically sync when changes are detected
- **Prune**: Remove resources that are no longer defined in Git
- **Self Heal**: Automatically sync if cluster state drifts from Git

## Adding New Applications

1. Create a new directory: `apps/<app-name>/`
2. Add your Kubernetes manifests to the directory
3. Create an `application.yaml` file with Argo CD Application resource
4. Commit and push to main branch
5. Argo CD will automatically detect and sync the new application

## Example Application Template

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: <app-name>
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/TamilTintin/tintin-argocd-applications
    targetRevision: HEAD
    path: apps/<app-name>
  destination:
    server: https://kubernetes.default.svc
    namespace: <namespace>
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

## Troubleshooting

### Application not syncing
```bash
argocd app sync <app-name>
```

### Check application details
```bash
argocd app describe <app-name>
```

### View application logs
```bash
kubectl logs -n argocd deployment/argocd-application-controller
```

## References

- [Argo CD Documentation](https://argo-cd.readthedocs.io/)
- [Argo CD Application CRD](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications)
- [Argo CD Sync Policies](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/)

## License

MIT License

## Contact

For questions or issues, please open a GitHub issue in this repository.

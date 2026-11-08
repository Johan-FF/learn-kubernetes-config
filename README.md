# learn-kubernetes-config

Kubernetes configuration for the DevOps with Kubernetes course project. The application code lives
in [learn-kubernetes](https://github.com/Johan-FF/learn-kubernetes); this repository holds only
what the cluster needs.

## Contents

| Directory | Purpose |
| --- | --- |
| `project/` | Kustomize base and overlays for the course project (GKE and local) |
| `gke/` | Manifests for the chapter 3 GKE exercises |
| `argocd/` | ArgoCD Applications that keep the cluster in sync with this repository |

## Why a separate repository

Exercise 4.10 asks for the split, and it has practical value:

- A change to a manifest does not need an application image rebuild, and a code change does not
  touch the deployment description.
- ArgoCD watches only this repository, so a push to the application code does not trigger a
  reconcile loop that has nothing to reconcile.
- Access can be scoped: the application repository needs CI write access to the registry, this one
  needs only read access for ArgoCD.

The application repository still builds and pushes the images; it does not apply manifests any more.

## Bootstrap

```bash
kubectl apply -f argocd/
kubectl -n argocd get applications
```
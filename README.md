# fastkubefy-gitops

GitOps repository for the [`fastkubefy`](https://github.com/JCPurger/fastkubefy) application.

This repository exists **only to be watched by ArgoCD**. It holds the Kubernetes manifests and acts as the **single source of truth** for everything that runs in the cluster: whatever is committed here is what ArgoCD reconciles into Kubernetes.

## Purpose

- Declaratively describe the desired state of the workloads in Kubernetes.
- Be the only place changes to the cluster are made — no `kubectl apply` by hand.
- Let ArgoCD continuously sync the cluster with the contents of this repo.

## How it works

1. A change is committed to this repository (e.g. a new image tag, a tweak to replicas, a new resource).
2. ArgoCD detects the change on the tracked branch.
3. ArgoCD applies the manifests to the cluster, bringing the live state in line with Git.

```
   Developer ──push──▶ Git (this repo) ──watch──▶ ArgoCD ──apply──▶ Kubernetes
                                                              ▲
                                                              │
                                                  source of truth = Git
```

## Repository layout

```
apps/
└── fastkubefy/          # Kustomize app tracked by ArgoCD
    ├── deployment.yml
    ├── service.yml
    └── kustomization.yml
```

## Conventions

- This repo contains **configuration only** — no application source code.
- Application code lives in the [`fastkubefy`](https://github.com/JCPurger/fastkubefy) repository; its CI publishes container images that are referenced here by tag.
- Do **not** change cluster state outside of this repository. If it isn't in Git, it shouldn't exist in the cluster.

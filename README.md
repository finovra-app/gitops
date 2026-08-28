# Finovra — GitOps Config Repo

This repo holds everything ArgoCD needs for Finovra: the `Application`
definition and the Kubernetes manifests it deploys. It's deliberately
separate from [`webapp`](https://github.com/finovra-app/webapp),
which holds the application source code and Dockerfiles — a standard
"app repo vs. config repo" split used by most real GitOps teams.

## Layout

```
gitops/
├── apps/
│   └── finovra.yaml              # the ArgoCD Application resource
└── k8s/
    └── plain-manifests/
        ├── dashboard/             # Deployment + Service
        ├── accounts-service/      # Deployment + Service
        ├── insurance-service/     # Deployment + Service
        ├── investments-service/   # Deployment + Service
        └── loans-service/         # Deployment + Service
```

## Why two repos?

- **`webapp`** changes when the *application* changes — new code, a new
  Docker image tag. Its natural audience later in the course is CI (Module 6).
- **`gitops`** changes when the *desired state of the cluster* changes — a new
  manifest, a replica count, a sync policy tweak. Its audience is ArgoCD.

Keeping them separate means an image build never has to touch a manifest, and
a manifest edit never has to touch application code — each repo has one job.

## Usage

Apply the Application once:

```bash
kubectl apply -f apps/finovra.yaml
```

From there, ArgoCD watches `k8s/plain-manifests/` in this repo. Everything
else — sync, prune, self-heal — is covered in the course modules.

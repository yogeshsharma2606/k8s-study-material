# Lab 08 — Kustomize Packaging

Package the web app from Lab 01 using Kustomize overlays for different environments.

## Objectives

- Structure manifests as base + overlays
- Apply dev vs prod differences (replicas, image tag)
- Use `kubectl kustomize` and `kubectl apply -k`

## Steps

### 1. Preview dev overlay

```bash
kubectl kustomize labs/08-packaging/overlays/dev
```

### 2. Deploy dev environment

```bash
kubectl apply -k labs/08-packaging/overlays/dev
kubectl get deploy,svc -n study -l app=web
```

### 3. Preview prod overlay

```bash
kubectl kustomize labs/08-packaging/overlays/prod
```

Note: prod overlay sets 3 replicas and adds a `environment: production` label.

### 4. Switch to prod (optional)

```bash
kubectl apply -k labs/08-packaging/overlays/prod
kubectl get deploy web -n study
```

### 5. Helm alternative (awareness)

If Helm is installed, you could package similarly:

```bash
# helm create web-chart
# helm install web ./web-chart -f values-dev.yaml
```

This lab uses Kustomize as the primary tool (built into kubectl).

## Cleanup

```bash
kubectl delete -k labs/08-packaging/overlays/dev
# or: kubectl delete -k labs/08-packaging/overlays/prod
```

## Related Module

[Module 10 — Packaging & Delivery](../../modules/10-deploy-gitops.md)

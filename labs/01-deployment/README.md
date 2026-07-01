# Lab 01 — First Deployment + Service

Deploy a stateless web app and expose it inside the cluster.

## Objectives

- Create a Deployment with multiple replicas
- Expose it with a ClusterIP Service
- Access via port-forward and in-cluster DNS

## Steps

### 1. Apply manifests

```bash
kubectl apply -f labs/01-deployment/
```

### 2. Watch rollout

```bash
kubectl rollout status deployment/web -n study
kubectl get pods -n study -l app=web -o wide
```

### 3. Check Service and endpoints

```bash
kubectl get svc web -n study
kubectl get endpoints web -n study
```

### 4. Port-forward from your machine

```bash
kubectl port-forward svc/web 8080:80 -n study
```

Open http://localhost:8080 — you should see the nginx welcome page.

### 5. Test in-cluster DNS

```bash
kubectl run curl-test --rm -it --restart=Never --image=curlimages/curl:8.5.0 -n study -- curl -s http://web
```

### 6. Rolling update

```bash
kubectl set image deployment/web nginx=nginx:1.26-alpine -n study
kubectl rollout status deployment/web -n study
kubectl rollout history deployment/web -n study
```

### 7. Rollback

```bash
kubectl rollout undo deployment/web -n study
```

## Cleanup

```bash
kubectl delete -f labs/01-deployment/
```

## Related Module

[Module 3 — Workload Controllers](../../modules/03-workloads.md)

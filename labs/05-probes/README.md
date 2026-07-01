# Lab 05 — Probes + Resources

Configure health probes and resource limits. Break and fix misconfigurations.

## Objectives

- Configure liveness and readiness probes
- Observe probe failures in `kubectl describe`
- Compare resource requests vs limits with `kubectl top`

## Steps

### 1. Apply manifests

```bash
kubectl apply -f labs/05-probes/
```

### 2. Verify probes

```bash
kubectl describe pod -l app=probed-app -n study | findstr /i "Liveness Readiness"
# Linux/macOS: kubectl describe pod -l app=probed-app -n study | grep -A3 "Liveness\|Readiness"
```

### 3. Check metrics (requires metrics-server)

```bash
kubectl top pods -n study -l app=probed-app
```

> metrics-server per tool: **kind** — apply + `--kubelet-insecure-tls` patch (Lab 00); **minikube** — `minikube addons enable metrics-server`; **Rancher Desktop** — bundled with k3s (already available). If `kubectl top` errors, metrics-server isn't ready yet.

### 4. Break readiness (wrong path)

```bash
kubectl patch deployment probed-app -n study --type='json' \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/readinessProbe/httpGet/path", "value": "/bad"}]'
kubectl get endpoints probed-app -n study
```

Endpoints should be empty — Service has no backends.

### 5. Fix readiness

```bash
kubectl patch deployment probed-app -n study --type='json' \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/readinessProbe/httpGet/path", "value": "/"}]'
kubectl get endpoints probed-app -n study
```

### 6. Observe PDB

```bash
kubectl get pdb -n study
```

## Cleanup

```bash
kubectl delete -f labs/05-probes/
```

## Related Module

[Module 7 — Health, Resources & Scaling](../../modules/07-health-scaling.md)

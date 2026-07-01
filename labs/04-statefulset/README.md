# Lab 04 — StatefulSet + PVC

Run a stateful application with stable identity and persistent storage.

## Objectives

- Deploy a StatefulSet with headless Service
- Observe stable Pod names and ordered creation
- Verify data persists across Pod restarts

## Steps

### 1. Apply manifests

```bash
kubectl apply -f labs/04-statefulset/
```

### 2. Watch ordered Pod creation

```bash
kubectl get pods -n study -l app=nginx-sts -w
```

Pods appear as `nginx-sts-0`, `nginx-sts-1`, `nginx-sts-2` sequentially.

### 3. Check PVCs

```bash
kubectl get pvc -n study
```

Each Pod has its own PVC from `volumeClaimTemplates`.

### 4. Write data to Pod 0

```bash
kubectl exec nginx-sts-0 -n study -- sh -c 'echo "data from pod-0" > /usr/share/nginx/html/index.html'
curl -H "Host: nginx-sts-0.nginx-headless.study.svc.cluster.local" http://localhost/ 2>/dev/null || \
kubectl exec nginx-sts-0 -n study -- cat /usr/share/nginx/html/index.html
```

### 5. Delete Pod 0 and verify data persists

```bash
kubectl delete pod nginx-sts-0 -n study
kubectl wait --for=condition=ready pod/nginx-sts-0 -n study --timeout=60s
kubectl exec nginx-sts-0 -n study -- cat /usr/share/nginx/html/index.html
```

### 6. DNS for StatefulSet

```bash
kubectl run dns-test --rm -it --restart=Never --image=busybox:1.36 -n study -- \
  nslookup nginx-headless.study.svc.cluster.local
```

## Cleanup

```bash
kubectl delete -f labs/04-statefulset/
# PVCs may remain — delete manually if needed:
kubectl delete pvc -n study -l app=nginx-sts
```

## Related Module

[Module 6 — Storage](../../modules/06-storage.md)

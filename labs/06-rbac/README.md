# Lab 06 — RBAC Least Privilege

Create a ServiceAccount with minimal permissions and verify access boundaries.

## Objectives

- Create Role, ServiceAccount, RoleBinding
- Verify allowed and denied API operations
- Run kubectl as the restricted ServiceAccount

## Steps

### 1. Apply manifests

```bash
kubectl apply -f labs/06-rbac/
```

### 2. Test permissions with auth can-i

```bash
# Allowed: list pods
kubectl auth can-i list pods --as=system:serviceaccount:study:reader-sa -n study

# Denied: delete pods
kubectl auth can-i delete pods --as=system:serviceaccount:study:reader-sa -n study

# Denied: access other namespace
kubectl auth can-i list pods --as=system:serviceaccount:study:reader-sa -n default
```

### 3. Run as ServiceAccount

```bash
kubectl run rbac-test --rm -it --restart=Never \
  --serviceaccount=reader-sa \
  --image=bitnami/kubectl:1.29 \
  -n study -- kubectl get pods
```

### 4. Verify delete is denied

```bash
kubectl run rbac-test-delete --rm -it --restart=Never \
  --serviceaccount=reader-sa \
  --image=bitnami/kubectl:1.29 \
  -n study -- kubectl delete pod --all
```

Should show "Forbidden" error.

## Cleanup

```bash
kubectl delete -f labs/06-rbac/
```

## Related Module

[Module 8 — Security](../../modules/08-security.md)

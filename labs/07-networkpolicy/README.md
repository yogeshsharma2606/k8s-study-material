# Lab 07 — NetworkPolicy

Implement default-deny and explicit allow rules for Pod traffic.

## Objectives

- Apply default-deny NetworkPolicy
- Allow specific ingress between labeled Pods
- Verify blocked and allowed connections

## Steps

### 1. Deploy frontend and backend

```bash
kubectl apply -f labs/07-networkpolicy/apps.yaml
```

### 2. Verify connectivity (before policy)

```bash
kubectl exec frontend -n study -- wget -qO- --timeout=3 http://backend:5678
```

### 3. Apply default-deny

```bash
kubectl apply -f labs/07-networkpolicy/deny-all.yaml
```

### 4. Verify connectivity blocked

```bash
kubectl exec frontend -n study -- wget -qO- --timeout=3 http://backend:5678
```

Should fail (timeout or connection refused).

### 5. Apply allow rule

```bash
kubectl apply -f labs/07-networkpolicy/allow-frontend-to-backend.yaml
```

### 6. Verify connectivity restored

```bash
kubectl exec frontend -n study -- wget -qO- --timeout=3 http://backend:5678
```

### 7. Verify unrelated pods still blocked

```bash
kubectl run outsider --rm -it --restart=Never --image=busybox:1.36 -n study -- \
  wget -qO- --timeout=3 http://backend:5678
```

Should fail.

## Cleanup

```bash
kubectl delete -f labs/07-networkpolicy/
```

## Related Module

[Module 4 — Networking](../../modules/04-networking.md)

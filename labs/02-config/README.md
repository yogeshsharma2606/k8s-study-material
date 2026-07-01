# Lab 02 — ConfigMap + Secret Injection

Learn how to inject configuration and secrets into Pods.

## Objectives

- Create ConfigMap and Secret
- Inject via environment variables and volume mounts
- Observe hot-reload behavior for mounted ConfigMaps

## Steps

### 1. Apply manifests

```bash
kubectl apply -f labs/02-config/
```

### 2. Verify env vars

```bash
kubectl exec config-demo -n study -- env | grep APP_
```

### 3. Verify volume mount

```bash
kubectl exec config-demo -n study -- cat /etc/config/config.json
```

### 4. Update ConfigMap (hot reload demo)

```bash
kubectl patch configmap app-config -n study --type merge -p '{"data":{"APP_MODE":"staging"}}'
# Wait ~60 seconds for kubelet sync, then:
kubectl exec config-demo -n study -- cat /etc/config/APP_MODE
```

Note: env vars from `envFrom` do **not** update without Pod restart.

### 5. Restart to pick up env changes

```bash
kubectl rollout restart deployment/config-demo -n study
```

## Cleanup

```bash
kubectl delete -f labs/02-config/
```

## Related Module

[Module 5 — Configuration & Secrets](../../modules/05-config-secrets.md)

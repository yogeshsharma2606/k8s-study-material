# Debugging Playbook

Systematic troubleshooting for Kubernetes workloads.

## Step 0 — Gather Context

```bash
kubectl config current-context
kubectl get pods -n <ns> -o wide
kubectl get events -n <ns> --sort-by='.lastTimestamp' | tail -20
```

## Symptom: Pod Stuck in Pending

**Likely causes:** Insufficient CPU/memory, PVC not bound, node taints, affinity rules, no nodes.

```bash
kubectl describe pod <pod> -n <ns>
# Look for: FailedScheduling events

kubectl get pvc -n <ns>
kubectl describe pvc <claim> -n <ns>
kubectl describe nodes | grep -A5 "Allocated resources"
kubectl get nodes --show-labels
```

**Fix paths:**
- Reduce resource requests or add nodes
- Fix StorageClass / PVC binding
- Add tolerations or fix nodeSelector/affinity

---

## Symptom: CrashLoopBackOff

**Likely causes:** Application error, missing config, wrong command, probe killing container.

```bash
kubectl logs <pod> -n <ns>
kubectl logs <pod> -n <ns> --previous
kubectl describe pod <pod> -n <ns>
# Check: Liveness probe, Exit Code, Last State
```

**Fix paths:**
- Fix app bug or config (ConfigMap/Secret)
- Adjust probe `initialDelaySeconds` / `failureThreshold`
- Use startup probe for slow-start apps

---

## Symptom: ImagePullBackOff / ErrImagePull

**Likely causes:** Wrong image tag, private registry without pull secret, typo.

```bash
kubectl describe pod <pod> -n <ns>
# Look for: Failed to pull image

kubectl get secret -n <ns>
```

**Fix paths:**
- Verify image name and tag exist
- Create `imagePullSecrets` and reference in Pod spec
- Check registry credentials

---

## Symptom: OOMKilled

**Likely causes:** Memory limit too low, memory leak.

```bash
kubectl describe pod <pod> -n <ns>
# Look for: Reason: OOMKilled

kubectl top pod <pod> -n <ns>
```

**Fix paths:**
- Increase `resources.limits.memory`
- Fix memory leak in application
- Ensure requests reflect actual usage

---

## Symptom: Running but Not Ready (0/N)

**Likely causes:** Readiness probe failing, app not listening on probe port.

```bash
kubectl describe pod <pod> -n <ns>
kubectl logs <pod> -n <ns>
kubectl exec <pod> -n <ns> -- wget -qO- localhost:<port><path>
kubectl get endpoints <svc> -n <ns>
```

**Fix paths:**
- Fix readiness probe path/port
- Ensure app binds to correct port before accepting traffic

---

## Symptom: Service Unreachable

**Likely causes:** Selector mismatch, no ready endpoints, NetworkPolicy blocking, wrong port.

```bash
kubectl get svc <svc> -n <ns>
kubectl get endpoints <svc> -n <ns>
kubectl get pods -n <ns> --show-labels
kubectl describe svc <svc> -n <ns>
kubectl get networkpolicy -n <ns>
```

**Debug checklist:**
1. Pod labels match Service `selector`?
2. Endpoints list has Pod IPs?
3. Pods are Ready?
4. `targetPort` matches container port?
5. NetworkPolicy allows traffic?

```bash
# Test from another pod
kubectl run debug --rm -it --image=curlimages/curl -n <ns> -- curl -v http://<svc>:<port>
```

---

## Symptom: Ingress Returns 404 or 502

```bash
kubectl describe ingress <name> -n <ns>
kubectl get ingressclass
kubectl logs -n ingress-nginx -l app.kubernetes.io/component=controller --tail=30
kubectl get endpoints -n <ns>
```

**Fix paths:**
- Verify `ingressClassName` matches installed controller
- Check path rules and backend service port
- Ensure backend Pods are Ready

---

## Symptom: Deployment Rollout Stuck

```bash
kubectl rollout status deployment/<name> -n <ns>
kubectl get rs -n <ns>
kubectl describe deploy <name> -n <ns>
```

**Fix paths:**
- Check new Pod errors (image, probes, resources)
- Adjust `maxUnavailable` / `maxSurge`
- `kubectl rollout undo` if needed

---

## Symptom: Permission Denied (RBAC)

```bash
kubectl auth can-i <verb> <resource> -n <ns>
kubectl auth can-i <verb> <resource> --as=system:serviceaccount:<ns>:<sa> -n <ns>
kubectl describe rolebinding -n <ns>
```

---

## Quick Reference Table

| Status | First Command |
|--------|---------------|
| Pending | `kubectl describe pod` |
| CrashLoopBackOff | `kubectl logs --previous` |
| ImagePullBackOff | `kubectl describe pod` |
| OOMKilled | `kubectl describe pod` |
| Not Ready | `kubectl describe pod` + check probes |
| 502 from Service | `kubectl get endpoints` |
| 403 from API | `kubectl auth can-i` |

## Emergency Commands

```bash
# Force delete stuck pod
kubectl delete pod <pod> -n <ns> --grace-period=0 --force

# Restart deployment
kubectl rollout restart deployment/<name> -n <ns>

# Rollback deployment
kubectl rollout undo deployment/<name> -n <ns>

# Ephemeral debug container (K8s 1.23+)
kubectl debug -it <pod> -n <ns> --image=busybox --target=<container> -- sh
```

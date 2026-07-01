# kubectl Essentials

Quick reference for daily Kubernetes operations.

## Context & Cluster

```bash
kubectl config current-context
kubectl config get-contexts
kubectl config use-context kind-k8s-study   # or: minikube | rancher-desktop
kubectl cluster-info
kubectl get nodes -o wide
```

## Namespaces

```bash
kubectl get ns
kubectl create ns my-app
kubectl config set-context --current --namespace=study
```

## Get Resources

```bash
kubectl get pods -n study
kubectl get pods -n study -o wide
kubectl get pods -n study -l app=web
kubectl get deploy,svc,ingress -n study
kubectl get all -n study
kubectl api-resources
```

## Describe & Events

```bash
kubectl describe pod <name> -n study
kubectl describe deploy web -n study
kubectl get events -n study --sort-by='.lastTimestamp'
```

## Logs & Exec

```bash
kubectl logs <pod> -n study
kubectl logs <pod> -n study -c <container>
kubectl logs <pod> -n study --previous
kubectl logs -l app=web -n study --tail=50 -f
kubectl exec -it <pod> -n study -- sh
```

## Apply & Delete

```bash
kubectl apply -f manifest.yaml
kubectl apply -f labs/01-deployment/
kubectl apply -k overlays/dev
kubectl delete -f manifest.yaml
kubectl delete pod <name> -n study
```

## Deployments

```bash
kubectl rollout status deployment/web -n study
kubectl rollout history deployment/web -n study
kubectl rollout undo deployment/web -n study
kubectl set image deployment/web nginx=nginx:1.26-alpine -n study
kubectl scale deployment/web --replicas=5 -n study
```

## Services & Networking

```bash
kubectl get svc,endpoints -n study
kubectl port-forward svc/web 8080:80 -n study
kubectl run tmp --rm -it --image=curlimages/curl -n study -- curl http://web
```

## Config & Secrets

```bash
kubectl get configmap,secret -n study
kubectl describe configmap app-config -n study
kubectl get secret app-secret -n study -o yaml
```

## Storage

```bash
kubectl get pvc,pv,storageclass
kubectl describe pvc data-pvc -n study
```

## RBAC

```bash
kubectl auth can-i create pods -n study
kubectl auth can-i delete pods --as=system:serviceaccount:study:reader-sa -n study
kubectl get role,rolebinding,sa -n study
```

## Metrics

```bash
kubectl top nodes
kubectl top pods -n study
```

## Output Formats

```bash
kubectl get pod <name> -n study -o yaml
kubectl get pod <name> -n study -o jsonpath='{.status.phase}'
kubectl get pods -n study -w
```

## Useful Shortcuts

```bash
kubectl run demo --image=nginx:1.25-alpine -n study --dry-run=client -o yaml > pod.yaml
kubectl explain pod.spec.containers
kubectl explain deployment.spec.strategy
```

## Imperative vs Declarative

Prefer declarative:

```bash
# Good — reproducible
kubectl apply -f deployment.yaml

# Avoid in production — not in Git
kubectl run nginx --image=nginx
```

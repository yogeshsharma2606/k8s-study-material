# Lab 03 — Ingress Routing

Route HTTP traffic by hostname using an Ingress controller.

## Objectives

- Install/enable an ingress controller (per local tool)
- Create Ingress rules for path-based routing
- Access apps via host header on localhost

## Steps

### 1. Install/enable an ingress controller

Pick the path for your local tool. The lab manifest uses `ingressClassName: nginx`.

**kind** — apply the kind-specific ingress-nginx manifest:

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s
```

**minikube** — enable the addon, then run the tunnel in a separate terminal:

```bash
minikube addons enable ingress
minikube tunnel        # keep running; exposes ingress on 127.0.0.1
```

**Rancher Desktop** — k3s bundles **Traefik**. Either:
- install ingress-nginx (`helm install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx --create-namespace`), or
- change `ingressClassName: nginx` to `traefik` in [ingress.yaml](ingress.yaml) and use Traefik (already listening on localhost:80).

### 2. Deploy backend apps

```bash
kubectl apply -f labs/03-ingress/
```

### 3. Verify Ingress

```bash
kubectl get ingress -n study
```

### 4. Test routing

```bash
# App A (root path)
curl -H "Host: study.local" http://localhost/

# App B (/api path)
curl -H "Host: study.local" http://localhost/api
```

On Windows PowerShell:

```powershell
curl.exe -H "Host: study.local" http://localhost/
curl.exe -H "Host: study.local" http://localhost/api
```

> Reaching the host per tool: **kind** maps ports 80/443 to localhost via `kind-config.yaml`; **minikube** needs `minikube tunnel` running (or use `minikube ip` as the host); **Rancher Desktop** exposes the controller on localhost directly.

### 5. Debug if needed

```bash
kubectl describe ingress study-ingress -n study
kubectl logs -n ingress-nginx -l app.kubernetes.io/component=controller --tail=20
```

## Cleanup

```bash
kubectl delete -f labs/03-ingress/
```

## Related Module

[Module 4 — Networking](../../modules/04-networking.md)

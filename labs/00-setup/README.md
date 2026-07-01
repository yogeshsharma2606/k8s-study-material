# Lab 00 — Local Cluster Setup

Create a local Kubernetes cluster for all subsequent labs. Pick **one** of three tools — **kind**, **minikube**, or **Rancher Desktop**. The labs' manifests and `kubectl` commands are identical across tools; only bootstrap, context name, ingress, and metrics-server differ.

## Prerequisites

- A container engine (Docker Desktop, or Rancher Desktop bundles one)
- `kubectl` ([install](https://kubernetes.io/docs/tasks/tools/))
- One of: [kind](https://kind.sigs.k8s.io/), [minikube](https://minikube.sigs.k8s.io/), or [Rancher Desktop](https://rancherdesktop.io/)

## Which tool?

| | kind | minikube | Rancher Desktop |
|---|------|----------|-----------------|
| Distro | upstream in Docker | upstream in VM/Docker | k3s |
| Multi-node | yes (config) | limited | single node |
| Context name | `kind-k8s-study` | `minikube` | `rancher-desktop` |
| Ingress | apply nginx manifest | `addons enable ingress` | bundled Traefik / install nginx |
| metrics-server | apply + patch | `addons enable metrics-server` | bundled |
| GUI | no | dashboard | yes |

---

## Option A — kind

```bash
kind create cluster --name k8s-study --config labs/00-setup/kind-config.yaml
kubectl config use-context kind-k8s-study
kubectl get nodes
```

metrics-server (needed for `kubectl top` / HPA in Lab 05):

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl patch deployment metrics-server -n kube-system --type='json' \
  -p='[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
kubectl rollout status deployment/metrics-server -n kube-system
```

Teardown: `kind delete cluster --name k8s-study`

---

## Option B — minikube

```bash
minikube start --driver=docker
kubectl config use-context minikube
kubectl get nodes
```

Enable addons (instead of installing manifests):

```bash
minikube addons enable metrics-server
minikube addons enable ingress        # for Lab 03
```

Teardown: `minikube delete`

---

## Option C — Rancher Desktop

1. Open Rancher Desktop → **Preferences → Kubernetes** → enable Kubernetes (k3s). It installs `kubectl` and sets the context.
2. Optionally choose the Kubernetes version and container engine (containerd or dockerd/moby).

```bash
kubectl config use-context rancher-desktop
kubectl get nodes
```

- **metrics-server** ships with k3s — `kubectl top nodes` should work out of the box.
- **Ingress**: k3s bundles **Traefik**. For Lab 03 (which uses `ingressClassName: nginx`) either change the lab's class to `traefik`, or install ingress-nginx (see Lab 03 notes).

Teardown: disable Kubernetes in the Rancher Desktop UI (or **Troubleshooting → Reset Kubernetes**).

---

## Common — create the study namespace (all tools)

```bash
kubectl apply -f manifests/namespace-study.yaml
kubectl get ns study
```

## Verify your cluster

```bash
kubectl cluster-info
kubectl get nodes -o wide
kubectl top nodes        # works once metrics-server is ready
```

## Next

Proceed to [Lab 01 — Deployment + Service](../01-deployment/README.md).

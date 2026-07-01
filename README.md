# Kubernetes Senior Developer Study Guide

A cloud-agnostic, **interview-prep-depth** curriculum for developers who need senior-level Kubernetes: control-plane internals, scheduling, networking and storage mechanics, security, autoscaling, production patterns, hands-on labs, and a question bank. Runs on **kind**, **minikube**, or **Rancher Desktop**.

**Estimated study time:** ~45 hours over 4–5 weeks.

Every module is written to the same interview-prep template: **TL;DR → Concept → How It Really Works (internals + diagram) → Why/When/Trade-offs → Worked Scenario → Gotchas → Interview Q&A → Verify → Further Reading**.

---

## Prerequisites

| Tool | Purpose | Install (Windows / choco) |
|------|---------|---------------------------|
| Container engine | Runs cluster nodes | Docker Desktop, or Rancher Desktop (bundles one) |
| kubectl | Cluster CLI | `choco install kubernetes-cli` |
| A local cluster tool | Run K8s locally | kind / minikube / Rancher Desktop — see below |
| Helm (Lab 08, optional) | Package manager | `choco install kubernetes-helm` |
| Kustomize | Config layering | Bundled in kubectl (`kubectl kustomize`) |

You should be comfortable with containers, YAML, and basic networking (IP, DNS, HTTP).

---

## Choosing a Local Cluster Tool

All labs use the same manifests and `kubectl` commands. Only **bootstrap, context name, ingress, and metrics-server** differ per tool.

| Tool | K8s distro | Multi-node | Ingress | LoadBalancer | GUI | Best for |
|------|-----------|-----------|---------|--------------|-----|----------|
| **kind** | upstream in Docker | Yes (config) | apply nginx manifest | needs MetalLB/`cloud-provider-kind` | No | CI, multi-node, closest to upstream |
| **minikube** | upstream in VM/Docker | Limited | `addons enable ingress` | `minikube tunnel` | Dashboard | Beginners, addons convenience |
| **Rancher Desktop** | k3s | No (single) | bundled Traefik (or install nginx) | k3s ServiceLB (Klipper) | Yes | Desktop dev, all-in-one, no Docker Desktop license |

Pick one and the rest of the guide just works. Per-tool setup details are in [Lab 00](labs/00-setup/).

---

## Quick Start — Local Cluster

**kind**
```bash
kind create cluster --name k8s-study --config labs/00-setup/kind-config.yaml
kubectl config use-context kind-k8s-study
```

**minikube**
```bash
minikube start --driver=docker
kubectl config use-context minikube
```

**Rancher Desktop**
```text
Enable Kubernetes in the Rancher Desktop UI (Preferences > Kubernetes).
It installs k3s + kubectl and sets the context to "rancher-desktop".
```
```bash
kubectl config use-context rancher-desktop
```

**Then, for any tool:**
```bash
kubectl get nodes
kubectl apply -f manifests/namespace-study.yaml
```

**Teardown:** `kind delete cluster --name k8s-study` · `minikube delete` · disable Kubernetes in Rancher Desktop.

---

## Curriculum (15 modules)

### Foundation

| # | Module | Topics |
|---|--------|--------|
| 1 | [Architecture & Mental Model](modules/01-architecture.md) | Reconciliation loop, request pipeline, finalizers, owner refs |
| 2 | [Pods](modules/02-pods.md) | Pause container, lifecycle, termination sequence, CRI |
| 3 | [Workload Controllers](modules/03-workloads.md) | Deployment/RS rollout math, StatefulSet, DaemonSet, Job |

### Platform

| # | Module | Topics |
|---|--------|--------|
| 4 | [Networking](modules/04-networking.md) | kube-proxy iptables/IPVS/eBPF, EndpointSlices, DNS/ndots, NetworkPolicy |
| 5 | [Configuration & Secrets](modules/05-config-secrets.md) | Injection + reload, subPath, encryption-at-rest/KMS |
| 6 | [Storage](modules/06-storage.md) | CSI flow, access modes, binding/topology, reclaim policy |

### Production

| # | Module | Topics |
|---|--------|--------|
| 7 | [Health, Resources & Scaling](modules/07-health-scaling.md) | Probe state machine, QoS, HPA formula, PDB |
| 8 | [Security](modules/08-security.md) | authn/authz pipeline, RBAC, bound tokens, admission, PSS |
| 9 | [Observability & Debugging](modules/09-observability.md) | Logging/metrics pipelines, debugging tree, ephemeral containers |

### Senior Topics

| # | Module | Topics |
|---|--------|--------|
| 10 | [Packaging & Delivery](modules/10-deploy-gitops.md) | Kustomize vs Helm, GitOps, canary/blue-green |
| 11 | [Operators & CRDs](modules/11-operators-crds.md) | CRD versioning, reconcile loop, finalizers, build vs buy |
| 12 | [Production Patterns](modules/12-production-patterns.md) | 12-factor, zero-downtime, blast radius, anti-patterns |
| 13 | [Scheduling & Placement](modules/13-scheduling-placement.md) | Filter/score, affinity, taints, topology spread, preemption |
| 14 | [Control Plane Internals](modules/14-control-plane-internals.md) | etcd/Raft, watch cache, informers, leader election, backup/restore |
| 15 | [Autoscaling & Capacity](modules/15-autoscaling-capacity.md) | HPA/VPA/Cluster Autoscaler/KEDA, requests, eviction |

---

## Hands-On Labs

Complete in order; each lab has its own `README.md`. Setup steps note kind / minikube / Rancher Desktop differences.

| Lab | Folder | Topic |
|-----|--------|-------|
| 00 | [labs/00-setup](labs/00-setup/) | Local cluster setup (all three tools) |
| 01 | [labs/01-deployment](labs/01-deployment/) | Deployment + Service |
| 02 | [labs/02-config](labs/02-config/) | ConfigMap + Secret |
| 03 | [labs/03-ingress](labs/03-ingress/) | Ingress routing |
| 04 | [labs/04-statefulset](labs/04-statefulset/) | StatefulSet + PVC |
| 05 | [labs/05-probes](labs/05-probes/) | Probes + resources |
| 06 | [labs/06-rbac](labs/06-rbac/) | RBAC least privilege |
| 07 | [labs/07-networkpolicy](labs/07-networkpolicy/) | NetworkPolicy |
| 08 | [labs/08-packaging](labs/08-packaging/) | Kustomize packaging |

---

## Cheatsheets

- [Interview question bank](cheatsheets/interview-questions.md) — ~70 Q&A + scenario prompts
- [kubectl essentials](cheatsheets/kubectl-essentials.md)
- [Debugging playbook](cheatsheets/debugging-playbook.md)

---

## Suggested Schedule

| Week | Modules | Labs | Hours |
|------|---------|------|-------|
| 1 | 1–3 | 00–01 | ~8 |
| 2 | 4–6 | 02–04 | ~10 |
| 3 | 7–9 | 05–07 | ~10 |
| 4 | 10–12 | 08 | ~10 |
| 5 | 13–15 + interview bank | review | ~8 |

---

## Interactive Canvas

Open the companion Canvas beside the chat for architecture diagrams, module navigation, comparison tables, the debugging playbook, and a progress tracker:

`C:\Users\yosh0225\.cursor\projects\empty-window\canvases\k8s-senior-study-guide.canvas.tsx`

---

## How Each Module Is Structured

1. **TL;DR** — the mental model in a few lines
2. **Concept** — plain-language explanation
3. **How It Really Works** — internals with a diagram
4. **Why / When / Trade-offs** — senior decision-making
5. **Worked Scenario** — a realistic situation end-to-end
6. **Gotchas & Failure Modes** — what bites people
7. **Interview Q&A** — sharp answers + follow-ups
8. **Verify** — kubectl commands
9. **Further Reading** — official docs

---

## Official References

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [kind](https://kind.sigs.k8s.io/docs/user/quick-start/) · [minikube](https://minikube.sigs.k8s.io/docs/start/) · [Rancher Desktop](https://docs.rancherdesktop.io/)

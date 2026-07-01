# Kubernetes Senior Interview Question Bank

~70 questions with concise model answers and the **follow-up** an interviewer usually asks. Grouped by topic. Practice saying the answer out loud in 30-60 seconds. Links point to the module with the full treatment.

Legend: **A** = model answer · **↳** = likely follow-up.

---

## Architecture & Control Plane ([M1](../modules/01-architecture.md), [M14](../modules/14-control-plane-internals.md))

**1. What happens when you `kubectl apply` a Deployment?**
A: API server runs authn → authz → mutating admission → validation → validating admission, persists to etcd. The Deployment controller creates a ReplicaSet, which creates Pods; the scheduler binds each Pod to a node; the kubelet starts containers via CRI. Each step is an independent reconcile loop.
↳ *Where could a mutating webhook change the object?* Before persistence — it can inject sidecars/defaults.

**2. What does "level-triggered" mean here and why use it?**
A: Controllers act on observed current vs desired state, not on individual events, so missed/duplicate events self-heal on the next observation/resync. It makes reconciliation idempotent and the system self-healing.

**3. Why is everything declarative?**
A: You store intent; controllers continuously converge actual to desired. Same manifest applied repeatedly converges identically, and the system recovers from failures without manual steps.

**4. What is etcd and how does Raft keep it consistent?**
A: The strongly-consistent KV store of all cluster state. Raft elects a leader and commits writes only after quorum `(N/2)+1` acknowledges — so run odd counts (3 tolerates 1, 5 tolerates 2).
↳ *Why not 4 members?* Same fault tolerance as 3 but more cost/latency.

**5. Why does only the API server talk to etcd?**
A: It centralizes authn/authz/admission/validation and serves a watch cache, so thousands of watchers read from it instead of overloading etcd.

**6. Explain the informer/workqueue pattern.**
A: LIST to fill a local cache, WATCH for deltas; reads use the cache (lister); changes enqueue keys onto a dedup/rate-limited/backoff workqueue; a worker runs idempotent Reconcile; periodic resync re-drives state.

**7. What is optimistic concurrency in Kubernetes?**
A: Every object has a `resourceVersion`; updates are compare-and-swap. A stale write gets a 409 Conflict and the client re-reads and retries — how concurrent writers stay safe.

**8. What's a finalizer and when have you hit one?**
A: A key that blocks deletion until a controller cleans up and removes it. Classic case: a namespace stuck Terminating because the owning controller is gone.
↳ *How do you unblock it safely?* Fix/restore the controller; only force-remove the finalizer as a last resort knowing cleanup won't run.

**9. How does leader election work?**
A: HA components run multiple replicas but contend for a Lease; only the holder is active and renews it; if it dies another takes over — preventing duplicate controllers acting at once.

**10. If the control plane is down, do running apps stop?**
A: No — kubelets and kube-proxy run autonomously. You lose scheduling, scaling, self-healing, and API-driven changes until it recovers.

**11. How do you back up and restore a cluster?**
A: `etcdctl snapshot save` (and keep PKI); restore with `etcdctl snapshot restore` into a new data dir, point etcd at it, restart control plane. State rebuilds from the snapshot.

---

## Pods & Workloads ([M2](../modules/02-pods.md), [M3](../modules/03-workloads.md))

**12. What do containers in a Pod share?**
A: Network namespace (one IP, shared localhost) and IPC via the pause container, plus mounted volumes; optionally PID. Each keeps its own filesystem and cgroup limits.

**13. Describe Pod termination and how you avoid dropped requests.**
A: On delete, endpoint removal and shutdown happen in parallel: removed from EndpointSlices while `preStop` runs, then SIGTERM, then SIGKILL after the grace period. Because endpoint removal is eventually consistent, add a `preStop` sleep and handle SIGTERM to drain, with grace period > longest request.
↳ *Why does the race cause 5xx?* A proxy may still route to a Pod that already got SIGTERM.

**14. Init container vs sidecar?**
A: Init runs to completion sequentially before app containers (migrations, waits); sidecar runs for the Pod's lifetime alongside the app (log/metric shippers, proxies) — natively an init container with `restartPolicy: Always`.

**15. Running vs Ready?**
A: Running = containers up. Ready = readiness passes and the Pod is added to Service endpoints. Running-but-not-Ready gets no traffic.

**16. Why does a Deployment use a ReplicaSet?**
A: The pod-template-hash ReplicaSet enables controlled rollouts and instant rollback — each revision is its own RS; rollback scales the old RS up.

**17. Explain maxSurge and maxUnavailable.**
A: During rollout, surge = Pods above desired allowed, unavailable = below desired allowed. `maxUnavailable:0`/`maxSurge:1` = zero-downtime by adding a Ready Pod before removing an old one, costing one extra Pod.

**18. Deployment vs StatefulSet vs DaemonSet?**
A: Deployment for interchangeable stateless replicas; StatefulSet for stable identity + per-replica storage + ordered ops (databases); DaemonSet for one Pod per node (agents).

**19. How do you canary a StatefulSet?**
A: Use rolling update `partition` — only ordinals ≥ partition update; lower it gradually validating each step.

**20. A rollout is stuck. Diagnose it.**
A: `rollout status`, then inspect the new RS's Pods, `describe`/`logs --previous` a failing Pod, `get events`. Usual causes: failing readiness, ImagePullBackOff, no room to surge. `rollout undo` to recover.

---

## Networking ([M4](../modules/04-networking.md))

**21. How does a request to a ClusterIP reach a Pod?**
A: The ClusterIP is virtual; kube-proxy (iptables/IPVS) or eBPF DNATs to a Ready endpoint Pod IP maintained by the EndpointSlice controller from the Service selector.

**22. iptables vs IPVS mode?**
A: iptables uses linear-ish probabilistic rules (O(n) updates, fine to a point); IPVS uses the kernel L4 LB with hash tables (O(1)) and real algorithms, scaling to thousands of Services.

**23. A Service has no endpoints — why?**
A: No Pod matches the selector (label mismatch), matching Pods aren't Ready, or none are running. Only Ready Pods enter EndpointSlices.
↳ *How do you confirm?* Compare Service selector to Pod labels; check readiness.

**24. Explain the ndots problem.**
A: resolv.conf `ndots:5` + search list means low-dot names try search domains first, so external lookups make several failed queries — latency + CoreDNS load. Fix: fully-qualify (trailing dot) or tune `dnsConfig`.

**25. When do you use a headless Service?**
A: When clients need direct Pod IPs, not a VIP — StatefulSet discovery, gRPC client-side LB, per-Pod addressing.

**26. How does NetworkPolicy default behavior work?**
A: All traffic allowed until a policy selects a Pod for a direction; then that direction is default-deny for that Pod and only listed peers are allowed. Additive; enforced by the CNI.

**27. ClusterIP vs NodePort vs LoadBalancer vs Ingress?**
A: ClusterIP = internal VIP; NodePort = port on every node; LoadBalancer = cloud LB per Service; Ingress = one L7 entry routing many Services behind a single LB.

**28. port vs targetPort?**
A: Clients connect to the Service `port`; it forwards to the container's `targetPort`.

---

## Config & Storage ([M5](../modules/05-config-secrets.md), [M6](../modules/06-storage.md))

**29. Are Secrets encrypted?**
A: No — base64 in etcd by default. Real protection = encryption-at-rest (ideally KMS) + tight RBAC on the Secret resource.

**30. Does updating a ConfigMap update a running Pod?**
A: Env vars: no (set at start; restart needed). Volume mounts: yes within ~1 min — except `subPath`, frozen at creation.

**31. Force a rollout on env-injected config change?**
A: Put a config checksum annotation on the Pod template (or use Kustomize `configMapGenerator` hashed names) so the template changes and rolls.

**32. Keep secrets out of Git in GitOps?**
A: Encrypt before Git (Sealed Secrets) or keep references and fetch at runtime (External Secrets Operator / Vault). Git never holds plaintext.

**33. Walk PVC → container reading the volume.**
A: PVC created → CSI controller CreateVolume + bound PV → Pod scheduled → CSI controller attaches volume to node → CSI node plugin stages + publishes (mounts) → container reads it.

**34. What does ReadWriteOnce restrict?**
A: RW mount by a single **node** (not Pod). Pods on the same node can share; a Pod on another node can't attach until detached — the multi-attach error.

**35. Why WaitForFirstConsumer?**
A: Delays provisioning until the Pod is scheduled so the volume lands in the right zone/topology; Immediate can deadlock on multi-zone.

**36. Prevent data loss on workload deletion?**
A: Reclaim policy Retain (or snapshots) and StatefulSet PVC retention; `Delete` removes the PV and underlying data with the PVC.

---

## Health, Scaling & Autoscaling ([M7](../modules/07-health-scaling.md), [M15](../modules/15-autoscaling-capacity.md))

**37. Liveness vs readiness?**
A: Liveness restarts a wedged container; readiness gates traffic via endpoints. Failing readiness = no traffic but still running; failing liveness = killed/restarted.

**38. Point of a startup probe?**
A: Suppresses liveness/readiness during a slow boot so startup doesn't trip liveness into a restart loop.

**39. CPU limit exceeded vs memory limit exceeded?**
A: CPU → throttled (slower, not killed); memory → OOMKilled and restarted. Memory limits protect; CPU limits bound latency/fairness.

**40. Derive HPA replicas.**
A: `ceil(current * currentMetric/targetMetric)`. 4 @ 80% CPU, target 50% → ceil(6.4)=7. Multiple metrics → take max; stabilization windows damp flapping.

**41. QoS classes and eviction order?**
A: Guaranteed (requests==limits all containers) evicted last; Burstable middle; BestEffort (nothing set) first under node pressure.

**42. HPA vs VPA vs Cluster Autoscaler?**
A: HPA → replica count by metrics; VPA → per-Pod requests/limits (right-size, recreates Pods); CA → node count from Pending Pods/idle nodes.
↳ *Why not HPA+VPA on CPU?* VPA changes requests = HPA's denominator → oscillation.

**43. What triggers Cluster Autoscaler scale-up?**
A: Unschedulable Pending Pods that would fit if a node were added; it reasons on requests, adds a node group node. Scale-down drains idle nodes honoring PDBs.

**44. When KEDA over plain HPA?**
A: Event-driven/bursty loads where the signal isn't CPU — queue depth, Kafka lag, cron — and you want scale-to-zero.

**45. How does a PDB interact with drain/scale-down?**
A: Eviction API honors PDBs; if eviction would violate min available it's refused and drain/scale-down waits. Covers only voluntary disruptions.

---

## Scheduling ([M13](../modules/13-scheduling-placement.md))

**46. How does the scheduler pick a node?**
A: Filter (feasible nodes: requests fit, affinity/selector match, taints tolerated, topology/volume ok) then Score (rank) then Bind. None feasible → Pending, maybe preemption.

**47. nodeSelector vs node affinity vs pod affinity?**
A: nodeSelector = simple hard node-label match; node affinity = richer node-label rules (required/preferred); pod affinity/anti-affinity = placement relative to other Pods within a topology domain.

**48. Taints/tolerations vs affinity?**
A: Taints repel Pods from a node unless tolerated (node-side keep-out); affinity attracts Pods to nodes (Pod-side). Dedicated hardware: taint node + toleration + affinity on intended Pods.

**49. Ensure replicas survive a zone failure?**
A: Topology spread constraints (`topologyKey: zone`, `maxSkew:1`) plus a hostname constraint — cheaper/clearer than anti-affinity.

**50. What is preemption?**
A: A high-PriorityClass Pending Pod can evict lower-priority Pods to schedule, guaranteeing critical workloads capacity under contention.

**51. A Pod is Pending — how do you find why?**
A: `kubectl describe pod` FailedScheduling event lists per-node rejection reasons (insufficient cpu/mem, untolerated taint, affinity mismatch, volume zone).

---

## Security ([M8](../modules/08-security.md))

**52. Request pipeline and where security sits?**
A: authn (who) → authz/RBAC (allowed?) → mutating then validating admission (incl. Pod Security Admission) → persist. Each is a distinct control.

**53. How does RBAC decide allow/deny?**
A: Allow-only, deny-by-default, no deny rules. Effective perms = union of bound Roles/ClusterRoles; ungranted = denied.

**54. Role vs ClusterRole; ClusterRole bound by RoleBinding?**
A: Role is namespaced; ClusterRole is cluster-scoped/reusable. A ClusterRole bound via RoleBinding applies only in that namespace — reuse with namespace scope.

**55. How do Pods authenticate to the API today?**
A: Projected bound ServiceAccount tokens — short-lived, audience-scoped, auto-rotated, tied to the Pod — replacing old long-lived Secret tokens.

**56. Pod Security Standards and enforcement?**
A: privileged/baseline/restricted, enforced by built-in Pod Security Admission via namespace labels (`enforce`/`audit`/`warn`). Restricted = non-root, drop caps, seccomp, no priv-esc.

**57. Production container hardening settings?**
A: `runAsNonRoot`, `readOnlyRootFilesystem`, `allowPrivilegeEscalation:false`, drop ALL caps, seccomp RuntimeDefault, dedicated minimal-RBAC SA, scanned digest-pinned distroless image.
↳ *Beyond PSA?* Kyverno/OPA Gatekeeper for registry allowlists, required labels, signature checks.

---

## Delivery & Operators ([M10](../modules/10-deploy-gitops.md), [M11](../modules/11-operators-crds.md))

**58. Kustomize vs Helm?**
A: Kustomize transforms plain YAML via bases/overlays/patches (no templating) — in-house apps, readable env diffs. Helm templates from values + manages release revisions/rollback — packaging/distribution. Often combined.

**59. GitOps vs CI push?**
A: GitOps runs an in-cluster controller reconciling live state to Git (pull) → drift detection/self-heal, full audit, PR-based change control, no cluster creds in CI.

**60. Canary vs blue-green?**
A: Blue-green runs v2 in parallel and switches all traffic at once (instant rollback, ~2x cost). Canary shifts a small % and ramps with metric analysis (safer, needs traffic-splitting).

**61. Instant rollback options?**
A: `kubectl rollout undo` (scales old RS up), `helm rollback`, `git revert` in GitOps, or blue-green Service switch.

**62. What is the operator pattern?**
A: A custom controller + custom resource (CRD) that automates an app's lifecycle (backup, failover, upgrades) by reconciling real resources to the CR's spec.

**63. CRD vs ConfigMap to extend behavior?**
A: CRD = first-class typed API (schema validation, RBAC, kubectl, status, watch); ConfigMap = opaque data. Use a CRD for new automated concepts.

**64. When NOT to build an operator?**
A: When it's just "apply manifests" (use Helm/Kustomize) or a mature community operator exists — adopt rather than maintain bespoke code.

---

## Debugging ([M9](../modules/09-observability.md), [debugging-playbook](debugging-playbook.md))

**65. CrashLoopBackOff — walk through it.**
A: `describe` (events, Last State, exit/OOM) → `logs --previous` → check probes (too strict?) and config. It's the kubelet backing off restarts of a container that keeps exiting.

**66. Distroless image, no shell — how to debug?**
A: `kubectl debug -it <pod> --image=busybox --target=<container>` — ephemeral container sharing the target's namespaces.

**67. metrics-server vs Prometheus?**
A: metrics-server = live CPU/mem for `top`/HPA, no history; Prometheus = stored time series for dashboards/alerts. Usually both.

**68. Intermittent 503s during deploys — causes?**
A: Rollout removing Ready Pods faster than replacements (`maxUnavailable` high), readiness flapping under load, SIGTERM/endpoint race without `preStop`, or a PDB blocking drain. Correlate events with the rollout timeline.

**69. Why are events unreliable for postmortems?**
A: They're API objects with ~1h TTL, not stored long-term — export them or they're gone.

---

## Open-ended scenario / design prompts

Practice structuring these out loud (assumptions → design → trade-offs → failure modes):

**S1. Design a zero-downtime deployment for a stateful-ish API.**
Cover: RollingUpdate `maxUnavailable:0`/`maxSurge:1`, readiness gating, `preStop` + SIGTERM drain, grace period > longest request, PDB; optional canary with metric analysis. Note DB migration ordering (backward-compatible schema, expand/contract).

**S2. A service intermittently returns 503s. Investigate end to end.**
Events → endpoints during rollout → readiness probe config → throttling (CPU limit) → PDB/`maxUnavailable` → upstream/DNS. Show the funnel and the fix per finding.

**S3. Make a 6-replica app survive node and zone failures.**
Topology spread across zone + hostname (`maxSkew:1`), PDB `maxUnavailable:1`, anti-affinity if needed, multi-AZ node groups, readiness for clean failover.

**S4. Secure a multi-tenant cluster.**
Namespace-per-tenant with ResourceQuota/LimitRange, default-deny NetworkPolicy + explicit allows, per-tenant RBAC + dedicated ServiceAccounts, Pod Security `restricted`, image policy via Kyverno; consider cluster-per-tenant if isolation must be hard.

**S5. Right-size and cut cost without hurting reliability.**
Set requests from real usage (VPA recommend/historical), Guaranteed QoS for critical, HPA for stateless, Cluster Autoscaler/Karpenter for nodes, PriorityClasses for preemption, remove over-provisioned limits causing throttling.

**S6. Roll out a risky change safely.**
GitOps with PR review, canary via Argo Rollouts/Flagger with automated metric analysis and auto-rollback, feature flags, observability (RED) dashboards, and a tested `git revert` path.

**S7. etcd lost quorum — recover and prevent recurrence.**
Restore from snapshot to rebuilt odd-sized members, verify endpoint health, then: odd member count, automated off-cluster snapshots, tested restore runbook, control-plane spread across failure domains.

**S8. A namespace won't delete.**
Identify the finalizer (namespace or contained objects), find/restore the responsible controller (often a CRD operator), let cleanup complete; force-clear the finalizer only as a last resort understanding cleanup won't run.

---

## How to use this bank

- First pass: answer from memory, then read the model answer.
- Second pass: do the **follow-ups** — that's where seniority shows.
- For scenarios, talk through assumptions and trade-offs, not just the happy path.
- Tie every answer back to the mechanism (reconcile loop, endpoints, requests, RBAC) — interviewers probe "why".

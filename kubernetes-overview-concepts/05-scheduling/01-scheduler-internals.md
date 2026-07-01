# Scheduler Internals

The Kubernetes Scheduler decides which node should run a newly created Pod.

Workflow:
1. Watch Pending Pods
2. Filter unsuitable nodes
3. Score remaining nodes
4. Select best node
5. Bind Pod

Scheduling considers:
- CPU & memory availability
- Taints & tolerations
- Affinity rules
- Topology constraints
- Resource requests

The scheduler never starts containers; it only assigns Pods to nodes.

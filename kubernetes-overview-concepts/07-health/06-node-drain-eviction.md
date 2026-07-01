# Node Drain & Eviction

Node draining prepares a node for maintenance.

Command:
kubectl drain <node>

Kubernetes:
- Marks the node unschedulable.
- Evicts Pods gracefully.
- Respects PodDisruptionBudgets.

After maintenance:
kubectl uncordon <node>

This allows new Pods to be scheduled on the node again.

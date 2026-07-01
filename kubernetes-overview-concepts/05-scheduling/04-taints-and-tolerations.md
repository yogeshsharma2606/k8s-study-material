# Taints & Tolerations

Taints are applied to Nodes to repel Pods.

Example:
gpu=true:NoSchedule

Pods require matching tolerations to be scheduled.

Effects:
- NoSchedule
- PreferNoSchedule
- NoExecute

Common use cases:
- Dedicated GPU nodes
- Database nodes
- Spot instances

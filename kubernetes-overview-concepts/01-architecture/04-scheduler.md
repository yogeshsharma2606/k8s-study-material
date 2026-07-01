# Scheduler

The scheduler decides **where** a Pod should run.

Workflow:
1. Watch for Pending Pods
2. Filter unsuitable nodes
3. Score remaining nodes
4. Select highest score
5. Bind Pod to node

Factors:
- CPU
- Memory
- Taints
- Affinity
- Topology spread

The scheduler never starts containers. Kubelet does.

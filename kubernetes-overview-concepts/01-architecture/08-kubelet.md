# Kubelet

Runs on every worker node.

Responsibilities:
- Register node
- Watch Pod assignments
- Ask container runtime to start containers
- Run health probes
- Report status

Kubelet never schedules Pods.

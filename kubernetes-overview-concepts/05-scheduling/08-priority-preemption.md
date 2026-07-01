# Priority & Preemption

PriorityClasses assign importance to Pods.

If resources are insufficient:
- Lower priority Pods may be evicted
- Higher priority Pods are scheduled

Use for:
- Critical infrastructure
- Monitoring
- Platform services

Avoid assigning high priority to every workload.

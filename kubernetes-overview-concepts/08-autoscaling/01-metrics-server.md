# Metrics Server

The Metrics Server collects CPU and memory usage from kubelets.

It provides resource metrics to:
- Horizontal Pod Autoscaler (HPA)
- kubectl top

It is not intended for long-term monitoring.

Typical flow:

Kubelet -> Metrics Server -> Kubernetes API -> HPA

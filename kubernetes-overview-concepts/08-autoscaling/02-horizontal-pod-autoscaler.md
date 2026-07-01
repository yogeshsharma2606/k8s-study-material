# Horizontal Pod Autoscaler (HPA)

HPA automatically changes the number of Pod replicas.

Common metrics:
- CPU utilization
- Memory utilization
- Custom metrics
- External metrics

Flow:
1. Read metrics.
2. Compare with target.
3. Increase or decrease replicas.

Suitable for stateless applications.

# Autoscaling Best Practices

- Define resource requests and limits.
- Install Metrics Server before using HPA.
- Avoid scaling purely on memory when possible.
- Use HPA for stateless applications.
- Use Cluster Autoscaler with HPA for elastic clusters.
- Use KEDA for event-driven workloads.
- Test scaling behavior under realistic load before production.

# KEDA

KEDA (Kubernetes Event-driven Autoscaling) scales workloads based on events.

Examples:
- Kafka lag
- RabbitMQ queue length
- Azure Service Bus
- Prometheus metrics

Flow:

External Event
      ↓
    KEDA
      ↓
     HPA
      ↓
Deployment

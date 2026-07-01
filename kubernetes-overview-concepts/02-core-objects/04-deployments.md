# Deployments

Deployments manage ReplicaSets and provide:

- Rolling updates
- Rollbacks
- Declarative scaling

Typical flow:

Deployment
    ↓
ReplicaSet
    ↓
Pods

Use Deployments for stateless applications.

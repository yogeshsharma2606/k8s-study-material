# Service Accounts

A ServiceAccount provides an identity for Pods.

Why?
Applications running inside Pods may need to communicate with the Kubernetes API.

Key points:
- One default ServiceAccount per namespace
- Token mounted into Pods
- Used together with RBAC

Flow:
Pod -> ServiceAccount -> API Server -> RBAC Authorization

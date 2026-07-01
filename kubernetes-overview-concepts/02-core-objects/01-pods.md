# Pods

## What is a Pod?

A Pod is the smallest deployable unit in Kubernetes.

A Pod can contain one or more tightly coupled containers that share:
- Network namespace (same IP)
- Storage volumes
- Lifecycle

## Why Pods?

Containers in the same Pod communicate over localhost and are scheduled together.

## Lifecycle

Pending → Running → Succeeded/Failed

## Common Use Cases

- Single application container
- Sidecar pattern (app + log collector)

## Best Practices

- One main application per Pod.
- Treat Pods as ephemeral; do not rely on Pod IPs.

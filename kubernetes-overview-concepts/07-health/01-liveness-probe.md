# Liveness Probe

## Purpose

Determines whether a container is still healthy.

If the probe fails repeatedly, Kubernetes restarts the container.

Probe Types:
- HTTP
- TCP
- Exec

Use liveness to recover from deadlocks or hung applications.

Do not use it to check external dependencies.

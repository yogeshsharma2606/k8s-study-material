# Chapter 4 – Kubernetes vs Docker

## Common Misconception

Kubernetes does **not** replace Docker.

Docker primarily builds and packages applications into images.

Kubernetes schedules and manages containers across many machines.

| Docker | Kubernetes |
|---------|------------|
| Build images | Schedule workloads |
| Run containers | Scale applications |
| Local development | Production orchestration |
| Single host | Multi-node cluster |

## Relationship

Typical workflow:

1. Build image.
2. Push image to registry.
3. Kubernetes pulls the image.
4. Pods are created from that image.

Modern Kubernetes commonly uses **containerd** as the runtime rather than Docker Engine itself.

## When to Use

Use Docker:
- Local development
- Building images

Use Kubernetes:
- High availability
- Production deployments
- Large-scale systems

## Summary

Docker and Kubernetes complement each other rather than compete.

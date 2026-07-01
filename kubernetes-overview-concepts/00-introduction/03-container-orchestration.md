# Chapter 3 – Container Orchestration

## What is Orchestration?

Running a single container is easy.

```bash
docker run nginx
```

Running thousands of containers across hundreds of servers is not.

Container orchestration automates:

- Scheduling
- Scaling
- Self-healing
- Networking
- Service discovery
- Rolling updates
- Secret and configuration management

## Why Orchestration?

Imagine an online shopping application with:

- Frontend
- User Service
- Payment Service
- Inventory Service
- PostgreSQL
- Redis

Questions arise:

- Which server should each service run on?
- What happens if a server crashes?
- How are new versions deployed?
- How do services find each other?

An orchestrator continuously answers these questions.

## Responsibilities

### Scheduling

Chooses the best machine based on CPU, memory and policies.

### Self-healing

Failed containers are restarted automatically.

### Scaling

Increase or decrease replicas based on demand.

### Service Discovery

Applications communicate using stable service names instead of changing IP addresses.

### Rolling Updates

Deploy new versions gradually while keeping the application available.

## Why Kubernetes?

Many orchestrators existed, but Kubernetes became the standard because of its declarative API, extensibility, portability and ecosystem.

## Summary

Docker runs containers.

An orchestrator manages containers across a cluster.

Kubernetes is the most widely adopted orchestrator.

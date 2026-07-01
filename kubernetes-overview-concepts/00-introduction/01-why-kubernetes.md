# Chapter 1 – Why Kubernetes?

> **Goal:** Understand why Kubernetes was created, what problems it solves, and why it became the de facto container orchestration platform.

---

# 1. Introduction

Imagine you have built a backend application and packaged it into a Docker image.

Running one container is easy:

```bash
docker run my-app
```

Now imagine your application becomes successful.

You suddenly need:

- 20 instances instead of 1
- Zero downtime deployments
- Automatic recovery when a server crashes
- Load balancing
- Secret management
- Configuration management
- Monitoring
- Service discovery

Docker alone does not solve these operational problems.

This is where Kubernetes comes in.

---

# 2. The Problem Before Kubernetes

Before orchestration platforms, operations teams managed servers manually.

Typical workflow:

1. Provision virtual machines.
2. Install dependencies.
3. Start applications.
4. Configure load balancers.
5. Monitor processes.
6. Restart failed applications manually.

As the number of services increased, this became difficult to manage.

---

# 3. Docker Solved One Problem

Docker standardized application packaging.

A Docker image contains:

- Application
- Runtime
- Libraries
- Dependencies

This solved the classic:

> "It works on my machine."

However, Docker focuses on **running containers**, not **managing thousands of containers**.

---

# 4. Why Kubernetes Was Created

Kubernetes automates container operations across a cluster of machines.

It continuously works toward the desired state you declare.

Example:

```yaml
replicas: 3
```

If one Pod crashes, Kubernetes automatically creates another to maintain three running replicas.

This design principle is called **declarative management**.

---

# 5. Problems Kubernetes Solves

## Self-healing

If a container crashes:

- Kubernetes detects the failure.
- The Pod is recreated automatically.

## Scaling

Instead of manually starting more containers:

```yaml
replicas: 10
```

Kubernetes ensures ten Pods are running.

## Rolling Updates

Applications can be upgraded without downtime by gradually replacing old Pods with new ones.

## Service Discovery

Pods receive new IP addresses whenever they are recreated.

Kubernetes provides Services and DNS so applications communicate using stable names instead of changing IP addresses.

## Load Balancing

Traffic is automatically distributed across healthy Pods.

---

# 6. Declarative vs Imperative Management

Imperative:

"Start three containers."

Declarative:

"I want three replicas."

Kubernetes continuously reconciles the actual cluster state with the desired state.

---

# 7. High-Level Architecture

```text
Developer
    │
kubectl
    │
API Server
    │
Control Plane
    │
Scheduler + Controllers
    │
Worker Nodes
    │
Pods
```

---

# 8. Real-World Example

Suppose an e-commerce application consists of:

- Frontend
- User Service
- Order Service
- Payment Service
- PostgreSQL
- Redis

Without Kubernetes:

- Manual deployment
- Manual scaling
- Manual failover

With Kubernetes:

- Automated scheduling
- Self-healing
- Service discovery
- Rolling updates
- Horizontal scaling

---

# 9. Common Misconceptions

**Myth:** Kubernetes replaces Docker.

**Reality:** Kubernetes is an orchestration platform. It manages containers produced by container runtimes such as containerd.

---

# 10. Interview Questions

1. Why was Kubernetes created?
2. What limitations of Docker does Kubernetes solve?
3. Explain declarative infrastructure.
4. What is self-healing?
5. Why is service discovery necessary?

---

# 11. Summary

Docker packages and runs containers.

Kubernetes manages containers at scale by providing scheduling, self-healing, networking, scaling, and declarative infrastructure.

This chapter establishes the motivation for everything else in Kubernetes.

In the next chapter we will study the history of Kubernetes and how it evolved from Google's Borg system.

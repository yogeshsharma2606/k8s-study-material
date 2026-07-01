# Chapter 2 – History of Kubernetes

> **Goal:** Understand where Kubernetes came from, why Google built Borg, and how those ideas evolved into Kubernetes.

---

# 1. Introduction

Kubernetes did not appear in isolation. It is the result of more than a decade of experience running massive distributed systems inside Google.

Long before Docker existed, Google was already scheduling billions of containers every week. The systems that enabled this were:

- Borg
- Omega
- Kubernetes

Understanding this history explains many Kubernetes design decisions.

---

# 2. The Challenge at Google

Google operated services such as Search, Gmail, Maps, and YouTube across thousands of servers.

Engineers needed to answer questions like:

- Where should an application run?
- What if a machine fails?
- How can applications be updated without downtime?
- How can resources be shared safely?

Managing servers manually was impossible.

---

# 3. Borg

Borg was Google's internal cluster manager.

Its responsibilities included:

- Scheduling workloads
- Recovering failed applications
- Packing workloads efficiently
- Resource isolation
- Rolling updates

Many Kubernetes concepts originated here:

| Borg | Kubernetes |
|-------|------------|
| Task | Pod |
| Job | Deployment / Job |
| Cell | Cluster |
| Scheduler | Scheduler |

Although Borg was never open sourced, Google published research papers describing its architecture.

---

# 4. Omega

Omega experimented with a shared-state architecture.

Instead of a single centralized scheduler, multiple schedulers worked from a common cluster state.

Many scheduling concepts later influenced Kubernetes.

---

# 5. Docker Changes Everything

Around 2013, Docker made containers accessible to developers.

Packaging applications became dramatically easier.

However, developers still lacked a production-grade orchestration platform.

Google decided to combine years of Borg experience with Docker's packaging model.

---

# 6. Birth of Kubernetes

In 2014 Google announced Kubernetes as an open-source project.

The name comes from the Greek word for **helmsman** or **pilot**, reflecting its purpose of steering containerized applications.

Soon after, Kubernetes became part of the Cloud Native Computing Foundation (CNCF).

---

# 7. Why Kubernetes Became Popular

Several factors contributed:

- Vendor-neutral
- Declarative API
- Strong community
- Extensible architecture
- Portable across cloud providers
- Rich ecosystem

Today it is the dominant orchestration platform.

---

# 8. Design Principles

Kubernetes inherited several ideas from Borg:

- Desired state management
- Declarative APIs
- Automatic reconciliation
- Self-healing
- Scheduling based on resources
- Controllers instead of manual operations

These principles appear repeatedly throughout the handbook.

---

# 9. Timeline

- 2003–2004: Borg developed internally.
- 2013: Docker released.
- 2014: Kubernetes announced.
- 2015: CNCF established.
- Today: Kubernetes powers workloads across public cloud, private cloud, and on-premises environments.

---

# 10. Interview Questions

1. What inspired Kubernetes?
2. What was Borg?
3. Why didn't Docker solve orchestration?
4. What ideas did Kubernetes inherit from Borg?
5. Why did Kubernetes become the industry standard?

---

# 11. Summary

Kubernetes is the open-source evolution of Google's experience operating large-scale distributed systems.

Its architecture is deeply influenced by Borg's concepts of scheduling, reconciliation, and declarative infrastructure.

The next chapter explains **container orchestration** itself—the problem Kubernetes was designed to solve.

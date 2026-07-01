# Chapter 5 – Kubernetes Cluster Overview

## What is a Cluster?

A Kubernetes cluster is a group of machines working together to run containerized applications.

It consists of:

- Control Plane
- Worker Nodes

```text
           kubectl
              │
        API Server
              │
   ┌──────────┴──────────┐
Scheduler   Controllers  etcd
              │
      ┌───────┴────────┐
      Worker Node 1    Worker Node 2
           │                │
         Kubelet         Kubelet
           │                │
          Pods            Pods
```

## Control Plane

Responsible for decisions:

- Scheduling
- Desired state
- Cluster management

Core components:

- API Server
- etcd
- Scheduler
- Controller Manager

## Worker Node

Runs workloads.

Components:

- Kubelet
- kube-proxy
- Container Runtime
- Pods

## Life of a Deployment

1. User executes `kubectl apply`.
2. API Server validates request.
3. Object stored in etcd.
4. Scheduler selects node.
5. Kubelet starts Pod.
6. Controllers ensure desired state.

The following chapters explain every component in detail.

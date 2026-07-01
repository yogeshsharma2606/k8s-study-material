# Control Plane

## What is it?

The control plane is the brain of Kubernetes. It makes cluster-wide decisions and maintains the desired state.

Components:
- API Server
- etcd
- Scheduler
- Controller Manager
- Cloud Controller Manager (optional)

## Responsibilities
- Accept API requests
- Store cluster state
- Schedule Pods
- Run reconciliation loops
- Detect failures

Flow:

User -> kubectl -> API Server -> etcd
                           |
                  Scheduler / Controllers
                           |
                      Worker Nodes

The control plane does **not** run application workloads. It manages them.

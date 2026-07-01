# Container Storage Interface (CSI)

CSI is the standard interface between Kubernetes and storage providers.

Responsibilities:
- Provision volumes
- Attach/Detach
- Mount/Unmount
- Snapshot support (driver dependent)

Examples:
- AWS EBS CSI
- Azure Disk CSI
- GCE PD CSI
- Ceph CSI

Flow:

PVC
  ↓
StorageClass
  ↓
CSI Driver
  ↓
Storage Backend

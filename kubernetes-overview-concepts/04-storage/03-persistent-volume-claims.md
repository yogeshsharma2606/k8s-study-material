# Persistent Volume Claims (PVC)

A PVC is a request for storage made by a workload.

Instead of selecting a specific disk, applications request:
- Capacity
- Access mode
- Storage class

The control plane binds a matching PV to the PVC.

Pods mount the PVC rather than referencing a PV directly.

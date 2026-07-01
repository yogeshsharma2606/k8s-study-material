# etcd

etcd is a distributed key-value database.

Purpose:
- Stores desired cluster state
- Stores ConfigMaps, Secrets, Pods, Deployments, CRDs

Properties:
- Strong consistency (Raft)
- Highly available
- Source of truth

If etcd is lost and backups do not exist, cluster state is lost.

# Persistent Volumes (PV)

A PersistentVolume is a cluster resource representing storage provisioned by an administrator or dynamically by a StorageClass.

Characteristics:
- Independent of Pods
- Can outlive Pods
- Backed by cloud disks, NFS, SAN, etc.

Lifecycle:
Available -> Bound -> Released -> Reclaimed

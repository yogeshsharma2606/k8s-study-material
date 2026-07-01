# Storage Classes

StorageClasses enable dynamic provisioning.

Instead of pre-creating PersistentVolumes, Kubernetes creates storage when a PVC is requested.

Benefits:
- Automation
- Multiple storage tiers
- Cloud integration

Examples:
- gp3
- standard
- premium-ssd

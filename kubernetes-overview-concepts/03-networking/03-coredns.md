# CoreDNS

CoreDNS provides DNS inside the cluster.

Example:

user-service.default.svc.cluster.local

Pods resolve service names instead of IP addresses.

CoreDNS watches the API Server and updates DNS records automatically.

# DaemonSets

A DaemonSet runs exactly one Pod on each eligible node.

Typical examples:
- Log collectors
- Monitoring agents
- Node exporters

When a new node joins the cluster, a new DaemonSet Pod is created automatically.

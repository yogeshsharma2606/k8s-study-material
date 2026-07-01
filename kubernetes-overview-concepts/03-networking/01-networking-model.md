# Networking Model

Kubernetes follows a flat networking model.

Rules:
- Every Pod gets its own IP.
- Pods can communicate without NAT.
- Nodes can reach Pods.
- Pods can reach other Pods.

Container Network Interface (CNI) plugins implement this model.

Popular CNIs:
- Calico
- Cilium
- Flannel

Traffic Flow:

Pod A -> CNI -> Network -> CNI -> Pod B

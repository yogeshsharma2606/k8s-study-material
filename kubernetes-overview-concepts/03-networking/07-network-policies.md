# Network Policies

Network Policies control which Pods can communicate.

Without policies, many CNI plugins allow all traffic by default.

Policies define:
- Allowed ingress
- Allowed egress

Useful for implementing zero-trust networking.

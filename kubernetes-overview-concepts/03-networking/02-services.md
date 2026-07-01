# Services

Services provide a stable endpoint for Pods.

Why?
Pod IPs change when Pods restart.

Types:
- ClusterIP
- NodePort
- LoadBalancer
- Headless

Flow:

Client -> Service -> kube-proxy -> Pod

Services use label selectors to find Pods.

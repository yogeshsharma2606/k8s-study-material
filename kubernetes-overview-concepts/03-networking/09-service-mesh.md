# Service Mesh

A Service Mesh manages service-to-service communication.

Capabilities:
- mTLS
- Retries
- Circuit breaking
- Traffic splitting
- Observability

Architecture:

App -> Sidecar Proxy -> Network -> Sidecar Proxy -> App

Popular meshes:
- Istio
- Linkerd

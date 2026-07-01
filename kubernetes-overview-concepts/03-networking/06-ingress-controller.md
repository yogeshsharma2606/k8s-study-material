# Ingress Controller

Ingress is only a resource definition.

The Ingress Controller watches Ingress resources and configures a reverse proxy.

Common implementations:
- NGINX
- Traefik
- HAProxy

Flow:

Internet -> LoadBalancer -> Ingress Controller -> Service -> Pod

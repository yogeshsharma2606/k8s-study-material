# values.yaml

values.yaml stores configurable values.

Example:
replicaCount: 3
image:
  repository: nginx
  tag: latest

Override values using:
- --set
- -f custom-values.yaml

This allows one chart to support dev, staging, and production environments.

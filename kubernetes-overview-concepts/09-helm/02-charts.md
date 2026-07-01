# Charts

A Chart is a package containing Kubernetes manifests.

Typical structure:

Chart.yaml
values.yaml
templates/
charts/

Chart.yaml stores metadata such as:
- Name
- Version
- Dependencies

Templates generate Kubernetes manifests during installation.

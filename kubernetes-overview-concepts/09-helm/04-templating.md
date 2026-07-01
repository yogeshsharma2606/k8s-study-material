# Helm Templates

Helm uses Go templates.

Templates reference values using:

{{ .Values }}

Common features:
- Variables
- if statements
- range loops
- Functions
- Named templates

Templates are rendered before resources are sent to the Kubernetes API.

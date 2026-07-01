# Security Context

SecurityContext defines privilege settings for Pods or containers.

Common options:
- runAsUser
- runAsGroup
- fsGroup
- readOnlyRootFilesystem
- allowPrivilegeEscalation
- capabilities

Best Practice:
Run containers as non-root whenever possible.

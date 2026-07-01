# Volumes

## Why Volumes?

Containers have ephemeral filesystems. Data written inside a container is lost when the container is recreated.

Volumes provide storage that can be shared between containers in a Pod or survive container restarts.

Common volume types:
- emptyDir
- hostPath
- configMap
- secret
- persistent volumes

Use volumes instead of storing important data inside container filesystems.

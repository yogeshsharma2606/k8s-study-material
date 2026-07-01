# Container Runtime

The runtime executes containers.

Examples:
- containerd
- CRI-O

Responsibilities:
- Pull images
- Create containers
- Start/Stop containers
- Report container status

Kubelet communicates using the Container Runtime Interface (CRI).

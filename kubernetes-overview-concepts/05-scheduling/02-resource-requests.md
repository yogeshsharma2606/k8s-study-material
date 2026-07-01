# Resource Requests

Requests specify the minimum CPU and memory required by a container.

Example:

cpu: 500m
memory: 512Mi

The scheduler uses requests when selecting a node.

Best Practice:
Always define requests for production workloads.

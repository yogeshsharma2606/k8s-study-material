# API Server

The API Server is the front door of Kubernetes.

Everything communicates through it.

Request lifecycle:
1. kubectl sends request
2. Authentication
3. Authorization
4. Admission Controllers
5. Validation
6. Persist object in etcd
7. Notify watchers

Why important?
- Single source for cluster operations
- REST API
- Watches and events

Interview:
Q: Can components communicate directly with etcd?
A: No. They communicate through the API Server.

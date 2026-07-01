# Controller Manager

Controllers continuously compare:

Desired State
vs
Actual State

Examples:
- Deployment Controller
- ReplicaSet Controller
- Job Controller
- Node Controller

This process is called the reconciliation loop.

Example:
Desired replicas = 3
Actual replicas = 2

Controller creates one more Pod.

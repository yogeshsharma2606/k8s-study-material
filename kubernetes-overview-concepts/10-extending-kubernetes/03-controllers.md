# Controllers

Controllers implement the reconciliation pattern.

Responsibilities:
- Watch Kubernetes resources
- Compare desired state with actual state
- Create, update or delete resources

Examples:
- Deployment Controller
- Job Controller
- Node Controller

Custom controllers extend this model for CRDs.

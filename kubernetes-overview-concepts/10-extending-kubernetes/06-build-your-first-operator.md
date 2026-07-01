# Build Your First Operator

High-level steps:

1. Define a CRD.
2. Install the CRD.
3. Create Go types for the API.
4. Implement a controller using controller-runtime or Operator SDK.
5. Watch Custom Resources.
6. Reconcile desired state.
7. Build a container image.
8. Deploy the controller to Kubernetes.
9. Create a Custom Resource and observe reconciliation.

Recommended tools:
- Kubebuilder
- Operator SDK
- controller-runtime

Best Practice:
Keep reconciliation idempotent so repeated executions always converge to the desired state.

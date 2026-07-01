# Reconciliation Loop

The reconciliation loop continuously ensures the actual cluster matches the desired state.

Flow:

User creates Custom Resource
        ↓
API Server stores object
        ↓
Controller receives event
        ↓
Compare desired vs actual state
        ↓
Create or update resources
        ↓
Repeat

This loop is fundamental to Kubernetes and all operators.

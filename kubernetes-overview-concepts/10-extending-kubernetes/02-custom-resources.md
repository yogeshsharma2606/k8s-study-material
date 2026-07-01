# Custom Resources (CRs)

A Custom Resource (CR) is an instance of a CRD.

Example:

kind: Database
metadata:
  name: orders-db

The CR represents the desired state. A controller observes it and performs the required actions.

CRDs define the schema; CRs contain user-specific configuration.

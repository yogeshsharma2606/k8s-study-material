# Pod Disruption Budget (PDB)

A PodDisruptionBudget limits how many Pods may be unavailable during voluntary disruptions.

Examples:
- Node maintenance
- Cluster upgrades
- Node drain

Common settings:
- minAvailable
- maxUnavailable

PDBs help maintain application availability during planned operations.

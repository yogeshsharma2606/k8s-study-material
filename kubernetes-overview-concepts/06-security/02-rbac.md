# RBAC (Role-Based Access Control)

RBAC controls what authenticated users or ServiceAccounts can do.

Resources:
- Role
- ClusterRole
- RoleBinding
- ClusterRoleBinding

Example:
A monitoring application may be allowed to list Pods but not delete them.

Principle:
Grant the least privilege required.

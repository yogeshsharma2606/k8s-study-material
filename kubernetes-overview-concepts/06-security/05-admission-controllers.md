# Admission Controllers

Admission Controllers intercept API requests after authentication and authorization.

Responsibilities:
- Validate resources
- Mutate resources
- Enforce policies

Examples:
- NamespaceLifecycle
- LimitRanger
- ResourceQuota
- MutatingAdmissionWebhook
- ValidatingAdmissionWebhook

Request Flow:

kubectl
   ↓
API Server
   ↓
Authentication
   ↓
Authorization
   ↓
Admission Controllers
   ↓
etcd

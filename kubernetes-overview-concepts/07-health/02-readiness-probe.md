# Readiness Probe

Readiness determines whether a Pod is ready to receive traffic.

If it fails:
- Pod is removed from Service endpoints.
- Container is NOT restarted.

Common use cases:
- Waiting for database connections
- Cache warm-up
- Application initialization

Readiness protects users from receiving traffic before the application is ready.

# Startup Probe

Startup probes are designed for slow-starting applications.

While the startup probe is running:
- Liveness checks are disabled.
- Readiness checks wait until startup succeeds.

Useful for:
- Spring Boot applications
- Large ML model loading
- Heavy initialization tasks

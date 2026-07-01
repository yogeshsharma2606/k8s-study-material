# CrashLoopBackOff

CrashLoopBackOff indicates a container repeatedly starts and crashes.

Common causes:
- Application errors
- Invalid configuration
- Missing secrets
- Failed dependencies
- Incorrect command

Troubleshooting:
1. kubectl logs
2. kubectl describe pod
3. Check events
4. Verify probes and configuration

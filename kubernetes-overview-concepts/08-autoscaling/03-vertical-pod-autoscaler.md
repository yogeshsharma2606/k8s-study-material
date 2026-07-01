# Vertical Pod Autoscaler (VPA)

VPA adjusts CPU and memory requests and limits for Pods.

Benefits:
- Better resource utilization
- Reduced manual tuning

Modes:
- Off
- Initial
- Auto

VPA is generally not used together with HPA on the same resource when both control CPU/memory.

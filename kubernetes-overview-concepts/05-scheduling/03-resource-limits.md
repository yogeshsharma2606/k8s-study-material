# Resource Limits

Limits define the maximum CPU or memory a container can consume.

Benefits:
- Prevent noisy neighbors
- Protect cluster resources

If a container exceeds:
- CPU: throttled
- Memory: may be OOMKilled

Requests and limits together determine Pod QoS.

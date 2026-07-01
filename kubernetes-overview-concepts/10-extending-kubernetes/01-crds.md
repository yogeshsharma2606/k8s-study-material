# Custom Resource Definitions (CRDs)

A Custom Resource Definition (CRD) extends the Kubernetes API by introducing a new resource type.

Why use CRDs?
- Model domain-specific resources
- Extend Kubernetes without modifying its source code

Example custom kinds:
- Database
- KafkaCluster
- SparkApplication

After a CRD is installed, the API server accepts requests for the new resource and stores objects in etcd.

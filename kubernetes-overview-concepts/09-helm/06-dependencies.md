# Chart Dependencies

Charts can depend on other charts.

Example:
Application chart
 ├── PostgreSQL
 └── Redis

Dependencies are defined in Chart.yaml and downloaded automatically.

This enables packaging complete application stacks.

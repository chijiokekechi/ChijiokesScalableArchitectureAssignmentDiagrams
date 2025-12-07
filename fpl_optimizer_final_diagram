```mermaid
flowchart LR
  %% Edge and ingress
  user[End User / Client] --> ga[AWS Global Accelerator]
  ga --> cf[Amazon CloudFront]
  cf -->|API| apigw[API Gateway (HTTP)]
  cf -->|Static UI| s3ui[S3 Static Website]

  %% Ingress to VPC / cluster
  apigw --> alb[Application Load Balancer]
  alb --> lattice[VPC Lattice Service Network]

  %% EKS services (multi-AZ)
  subgraph EKS[Amazon EKS Cluster]
    api[API Service]
    feature[Feature Service]
    optimizer[Optimizer Service]
    ingest[Ingest Service]
    adot[ADOT Collector]
  end

  lattice --> api
  lattice --> feature
  lattice --> optimizer
  lattice --> ingest

  %% State and events
  api --> redis[(ElastiCache for Redis)]
  feature --> redis
  optimizer --> redis
  ingest --> s3[S3 Artifacts & Snapshots]
  api --> aurora[(Aurora PostgreSQL)]
  feature --> aurora
  optimizer --> aurora
  ingest --> aurora
  ingest --> msk[(Amazon MSK Topics)]
  optimizer --> msk

  %% Observability
  adot --> xray[AWS X-Ray]
  adot --> amp[Amazon Managed Service for Prometheus]
  amp --> amg[Amazon Managed Grafana]

  %% Security (edge)
  cf --- waf[AWS WAF]
  cf --- acm[ACM Certificates]

  %% Notes
  classDef store fill:#f7f7f7,stroke:#333,stroke-width:1px
  class s3,aurora,redis,msk store
```

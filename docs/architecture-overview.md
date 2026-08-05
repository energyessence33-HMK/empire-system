# Architecture Overview

This diagram shows a high-level architecture for a typical web/mobile application: clients -> edge -> API layer -> services -> data/storage -> supporting infra (cache, message broker, monitoring, CI/CD).

```mermaid
flowchart LR
  %% Clients / Edge
  subgraph Clients
    Web[Web App<br/>Browser]
    Mobile[Mobile App]
  end

  CDN[CDN / WAF] -->|static assets| Web
  CDN --> Mobile
  CDN --> LB[Load Balancer]

  %% API Layer
  LB --> APIGW[API Gateway / Edge Proxy]
  APIGW --> Auth[Auth Service<br/>(OAuth / JWT)]
  APIGW --> BFF[Backend-for-Frontend]

  %% Application Services
  subgraph Services[Microservices]
    direction TB
    OrderSvc[Orders Service]
    ProductSvc[Products Service]
    BillingSvc[Billing Service]
    NotificationSvc[Notification Service]
  end

  APIGW -->|REST / gRPC| OrderSvc
  APIGW --> ProductSvc
  APIGW --> BillingSvc
  APIGW --> NotificationSvc

  %% Data & Storage
  DB[(Primary DB<br/>(Postgres / RDS))] 
  Replica[(Read Replica)]
  DB --> Replica

  ObjectStore[(Object Storage<br/>S3 / Blob)]
  Cache[(Redis / Memcached)]
  MQ[(Message Broker<br/>Kafka / RabbitMQ)]

  OrderSvc -->|read/write| DB
  ProductSvc -->|read-mostly| Replica
  BillingSvc -->|transactions| DB
  Services --> Cache
  Services --> MQ
  NotificationSvc --> ObjectStore
  MQ --> Worker[Background Workers / Consumers]
  Worker -->|persist results| DB

  %% Cross-cutting concerns
  Auth --> Cache
  Auth --> DB
  Services -.->|logs & traces| Logging[Logging & Tracing<br/>(ELK / Loki / Jaeger)]
  Services -.->|metrics| Monitoring[Monitoring<br/>(Prometheus + Grafana)]

  CI[CI/CD Pipeline]
  CI -->|build & deploy| Services
  CI -->|db migrations| DB

  ThirdParty[Third-Party APIs<br/>(payment, email, analytics)]
  BillingSvc --> ThirdParty
  NotificationSvc --> ThirdParty

  %% Notes
  classDef infra fill:#f9f9f9,stroke:#333,stroke-width:1px;
  class CDN,LB,APIGW,DB,Replica,ObjectStore,Cache,MQ,Logging,Monitoring,CI,ThirdParty infra;
```

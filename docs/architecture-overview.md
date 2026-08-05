# Architecture Overview

This file contains the Mermaid source and a generated SVG will be produced by CI so the diagram displays reliably on GitHub.

Mermaid source (also saved as docs/architecture-overview.mmd):

````mermaid
flowchart LR

subgraph Clients
    Web["Web App Browser"]
    Mobile["Mobile App"]
end

CDN["CDN / WAF"] -->|static assets| Web
CDN --> Mobile
CDN --> LB["Load Balancer"]

LB --> APIGW["API Gateway"]
APIGW --> Auth["Auth Service (OAuth / JWT)"]
APIGW --> BFF["Backend for Frontend"]

subgraph Services
    direction TB
    OrderSvc["Orders Service"]
    ProductSvc["Products Service"]
    BillingSvc["Billing Service"]
    NotificationSvc["Notification Service"]
end

APIGW -->|REST / gRPC| OrderSvc
APIGW --> ProductSvc
APIGW --> BillingSvc
APIGW --> NotificationSvc

DB[(Primary Database)]
Replica[(Read Replica)]
DB --> Replica

ObjectStore[(Object Storage)]
Cache[(Redis Cache)]
MQ[(Message Broker)]

OrderSvc --> DB
ProductSvc --> Replica
BillingSvc --> DB

OrderSvc --> Cache
ProductSvc --> Cache
BillingSvc --> Cache
NotificationSvc --> Cache

OrderSvc --> MQ
ProductSvc --> MQ
BillingSvc --> MQ
NotificationSvc --> MQ

NotificationSvc --> ObjectStore

MQ --> Worker["Background Workers"]

Worker --> DB

Auth --> Cache
Auth --> DB

OrderSvc -.-> Logging["Logging & Tracing"]
ProductSvc -.-> Logging
BillingSvc -.-> Logging
NotificationSvc -.-> Logging

OrderSvc -.-> Monitoring["Monitoring"]
ProductSvc -.-> Monitoring
BillingSvc -.-> Monitoring
NotificationSvc -.-> Monitoring

CI["CI/CD Pipeline"]

CI --> OrderSvc
CI --> ProductSvc
CI --> BillingSvc
CI --> NotificationSvc

CI --> DB

ThirdParty["Third Party APIs"]

BillingSvc --> ThirdParty
NotificationSvc --> ThirdParty
````

Notes:
- A GitHub Action in .github/workflows/render-mermaid.yml will render docs/architecture-overview.mmd to docs/architecture-overview.svg on push.
- If you prefer PNG instead of SVG, I can update the workflow to output PNG.

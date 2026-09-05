# Architecture 101

## Foundations and System Thinking: 2
### Latency vs Throughput
### Vertical vs Horizontal Scaling
### Stateful vs Stateless Systems
### Synchronous vs Asynchronous Processing
### Availability and Reliability
### Bottlenecks
### Coupling and Cohesion
### Monolith vs Modular Monolith vs Microservices
### Basic Networking
### Basic Database Thinking
### Requirements: Functional vs Nonfunctional
### Trade Off Thinking


## Data & storage: 2
### Relational Databases
### Indexes
### Normalization vs Denormalization
### Concurrency and Isolation
### Caching
### NoSQL Databases
### Search Engines
### Object Storage
### Replication
### Partitioning and Sharding
### OLTP vs OLAP
### Source of Truth
### Data Lifecycle


## Communication & messaging: 3
### Synchronous Communication
### REST vs gRPC
### Asynchronous Communication
### Message Queues
### Delivery Semantics
### Retries and Dead-letter Queues
### Pub/Sub
### Event Driven Architecture
### Kafka and Event Streaming
### Kafka vs RabbitMQ
### Ordering
### Backpressure and Buffering
### Eventual Consistency
### Integration and Apache Camel
### Service boundaries and Contracts
### Communication Failure Thinking


## Reliability & distributed systems: 4
### Partial Failure
### Timeouts
### Retries
### Idempotency
### Circuit Breakers
### Bulkheads
### Graceful Degradation
### Replication
### Consistency
### CAP Theorem
### Quorums
### Leader Election
### Distributed Locking
### Distributed Transactions
### Saga Pattern
### Workflow Engines
### Message Duplication and Ordering
### Split Brain
### Consensus
### Single Points of Failure
### Recovery Matters as Much as Prevention

## Scaling & infrastructure: 2
### Vertical Scaling
### Horizontal Scaling
### Load Balancing
### Reverse Proxies
### Stateless Application Design
### Containers
### Container Registries
### Docker Compose
### Health Checks
### AutoScaling
### Service Discovery
### Configuration Management
### Secrets Management
### Infrastructure as Code
### Deployment Strategies
### Kubernetes
### CDN and Edge Infrastructure
### Database Scaling
### Capacity Planning
### Cost as an Architectural Constraint

## Observability & operations: 2
### Monitoring vs Observability
### The Three Pillars: Metrics, Logs, Traces
### Metrics
### Logs
#### Structured Logging
#### Centralized Logging
### Distributed Tracing
### Spans and Traces
### Correlation IDs
### OpenTelemetry
### Percentiles
### RED Metrics
### USE Metrics
### Health Checks
### Alerting
### SLI, SLO and SLA
### Availability Percentages
### Error Budgets
### Business vs Technical Metrics
### Dashboards
### Kafka Consumer Lag
### Database Observability
### Capacity Trends
### Incident Response
### Postmorterms
### Runbooks
### Observability has a Cost


## Architecture patterns: 3
### Layered Architecture
### Hexagonal Architecture - Ports and Adapters
### Clean Architecture
### Modular Monolith
### Microservices
### Monolith vs Microservices
### Event Driven Architecture
### CQRS
### Event Sourcing
### Transactional Outbox
### Change Data Capture
### Saga Pattern
### API Gateway Pattern
### Backend for Frontend
### Anti Corruption Layer
### Strangler Fig Pattern
### Circuit Breaker
### Retry Pattern
### Bulkhead Pattern
### Cache Aside Pattern
### Database Per Service
### Shared Database Pattern
### Sidecar Pattern
### Service Mesh


## Architecture practice & trade-offs
### Start with Requirements, not Technology
### Quantify Scale
### Identify the Critical Paths
### Identify Invariants
### Identify what can be Eventually Consistent
### Draw the Simplest Architecture First
### Evolve Architecture from Constraints
### Separate Synchronous from Asynchronous Work
### Make Failure Scenarios Part of Design
### Trade-offs
### There's no Universally Best Database
### Decision Matrices
### Architecture Decision Records
### Architecture Diagrams
### C4 Model
### Design for the Team Too
### Build vs Buy
### Managed vs Self Hosted
### Cost Modelling
### Overengineering
### Underengineering
### Architecture Evolution
### Architecture Reviews
### Learn to Say "I don't know yet"
### Use the Same Process Everytime

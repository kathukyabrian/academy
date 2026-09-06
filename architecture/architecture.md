# Architecture 101

## Foundations and System Thinking: 2
### Latency vs Throughput
#### Latency
- latency is how long one operation takes
- if a request starts at 10:00:00.000 and response arrives at 10:00:00.200, the latency is 200ms
#### Throughput
- throughput is how much work a system can complete in a period of time.
- example:
    + for an API, 1,000 requests per second
    + for Kafka, 200,000 messages per second
- throughput is therefore amount of work completed per unit of time

---
- you can improve throughput without making an individual operation faster like in a supermarket:
    - with one cashier, each customer takes 1 minute
    - latency is 1 minute/customer
    - throughput is 1 customer/minute
    - make it 10 cashiers
    - latency remains the same
    - throughput is now 10 customers/minute

#### Queues
- assuming 1 customer takes 1 minute
- if 20 customer arrive simultaneously, they experience:
    - 19 minutes waiting + 1 minute processing = 20 minutes
- therefore:
    + total latency = queueing time + processing time
- therefore, a server might execute each request in 50ms, but users might experience 2 seconds because requests are waiting

#### Saturation
- assuming an application can sustain 1,000 tps
- when traffic is:
    + 500tps: okay
    + 800tps: okay
    + 950tps: getting busy
    + 1200tps: receiving work faster than you can process:
        + there's a backlog of 200tps
        + therefore, after 10 seconds, you'll have 2000 transactions waiting
        + latency explodes
- __as a system approaches saturation, latency can increase dramatically even though its maximum throughput barely increases__

#### Why Average Latency Can Lie
- suppose 100 requests have:
```
99 requests → 100 ms
1 request   → 10 seconds
```
- the average is around 199ms
- this doesn't tell the whole story
- a better way to look at it is using __percentiles__


#### p50, p95, p99
- suppose:
```
p50 = 100 ms
p95 = 300 ms
p99 = 2 seconds
```
- interpretation:
    - 50% of requests completed within 100ms - this is the median
    - 95% completed within 300ms, 5% took longer
    - 99% completed within 2s, 1% took longer
- the last 1% can matter ernomously

#### Why p99 matters:
- suppose your application handles:
```
1,000,000 requests/day
```
- if 1% have a terrible latency, it means
```
10,000 requests/day
```
- thats a big number of users having a bad experience
- this slow end of the distribution is called __tail latency__

#### Distributed Systems Amplify Latency
- imagine a request requires
```
API
 │
 ├── Customer Service    100 ms
 ├── Inventory Service   200 ms
 ├── Payment Service     500 ms
 └── Shipping Service    150 ms
```
- if they are called sequentially:
```
100 + 200 + 500 + 150
=
950 ms
```
- there's also other processing/network overhead

#### Parallelism can reduce latency
- suppose 3 independent calls take:
```
Customer       100 ms
Inventory      200 ms
Recommendations 300 ms
```
- sequential:
```
100 + 200 + 300 ≈ 600 ms
```

- parallel
```
           ┌── Customer       100ms
Request ───┼── Inventory      200ms
           └── Recommendations 300ms

roughly: 300ms
```
- concurrency can sometimes improve request latency.
- it may increase pressure on downstream systems

#### Batching
- suppose we need to write 1,000 records
- one by one might be inefficent
```
write
write
write
write
...
```
- batch can greatly improve throughput
```
[1,000 records]
       │
       ▼
    Database
```
- however, imagine the system waits until it has 1,000 messages before sending them:
    - first message might sit around waiting for the batch to fill up
- so:
```
larger batches
     ↓
potentially higher throughput
```
- but potentially:
```
larger batches
     ↓
more waiting
     ↓
higher latency
```
- this is a classic __throghput vs latency tradeoff__

#### Scaling often targets throughput
- suppose
```
App 1

capacity:
1,000 req/sec
```

- add instances:
```
             Load Balancer
            /      |      \
           ▼       ▼       ▼
        App 1    App 2    App 3
```

- system throughput might rise substantially
- however, a request is not 3 times faster
- therefore:
    + horizontal scaling commonly increases throughput more directly than it decreases per-request processing latency
- it can still reduce observed latency when it eliminates queueing

#### Throughput isn't just about request/seconds
- the unit depends on the system

|System|Unit|
|---|---|
|API|requests/sec|
|Kafka|messages/sec, MB/sec|
|Database|queries/sec, transactions/sec|

#### Latency has Layers:
- suppose:
```
User → API → DB → API → User
```

- total user latency might be:
```
DNS                  10 ms
TCP/TLS              30 ms
Network              40 ms
NGINX                 2 ms
Application          20 ms
Database            150 ms
Network return       40 ms
──────────────────────────
Total               ~292 ms
```
- architecture performance is often about identifying:
    + where is the time actually being spent?


#### Little's Law:
- for a stable system:
```
concurreny = throughput * latency
```
- suppose:
```
Throughput = 1,000 requests/sec

Average latency = 0.2 seconds
```

- then approximately:
```
1,000 × 0.2
=
200
``` 

- this means 200 requests are in the system concurrently on average

- if latency rises to 2 seconds while throughtput remains at 1000 request/sec

```
1,000 × 2
=
2,000
``` 

- 2000 requests are now in flight

- that can mean more:
    - threads
    - memory
    - connections
    - buffers

- explains why latency problems can cascade into reliability problems

#### Mindset Shift:
- when someone says: __our system is slow__, don't immediately conclude
```
we need more servers
```

- ask:
```
What is the latency?

p50?
p95?
p99?

At what throughput?

Is CPU saturated?

Is the database saturated?

Are requests queueing?

Is an external dependency slow?

Is it processing time or waiting time?

Is this all requests or only some?

Did throughput increase before latency increased?
```


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

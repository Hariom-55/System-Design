# Introduction to System Design

## 1. What is System Design?

**System Design** is the process of defining the architecture, components, data flow, communication mechanisms, and operational characteristics of a software system so that it can satisfy both its functional and non-functional requirements.

A system design answers questions such as:

- What components does the system need?
- How do those components communicate?
- Where is data stored?
- How will the system handle increasing traffic?
- How will we maintain performance?
- How will we make the system highly available and reliable?
- What happens when a component fails?
- How do we monitor and debug the system?

A useful way to think about system design is:

> **Requirements → Components → Communication → Data → Scaling → Reliability → Observability**

---

# 2. Why System Design Matters

A small application can often run on a single server with a single database. As the number of users, requests, and data grows, this architecture starts creating bottlenecks.

For example, consider a simple banking application.

Initially:

```text
User
  |
  v
Application Server
  |
  v
Database
```

This may work for a small number of users.

But suppose the application now has millions of users and thousands of requests arriving simultaneously.

Problems can appear:

- The application server may become overloaded.
- The database may receive too many requests.
- Some requests may become slow.
- A single server failure can make the entire system unavailable.
- Large amounts of repeated data may be unnecessarily fetched from the database.
- A sudden traffic spike can cause request queues to grow.

System design provides techniques to solve these problems.

---

# 3. First Step: Understand the Requirements

Before selecting technologies or drawing architecture diagrams, identify the requirements.

Requirements are broadly divided into:

1. **Functional Requirements**
2. **Non-Functional Requirements**

---

# 4. Functional Requirements

Functional requirements describe **what the system should do**.

They represent the features and business operations of the application.

### Example: E-Commerce Platform

Functional requirements could include:

1. User registration
2. User login
3. Product catalogue
4. Product search
5. Apply coupon code
6. Make payment
7. Place an order
8. Track an order

These requirements describe the actual behavior of the system.

### Key Question

> **WHAT should the system do?**

---

# 5. Non-Functional Requirements

Non-functional requirements describe **how well the system should operate**.

They define system qualities and operational constraints.

Common non-functional requirements include:

- Scalability
- Availability
- Reliability
- Performance
- Security
- Maintainability
- Observability
- Fault tolerance
- Data durability

### Example

Suppose an e-commerce system has the following requirements:

- Handle 1 million requests
- Response time below 200 ms for important APIs
- 99.9% availability
- Encrypt sensitive data
- Scale to 10× the current traffic
- Prevent data loss

These are non-functional requirements.

### Key Question

> **HOW should the system behave while performing its functions?**

---

# 6. Functional vs Non-Functional Requirements

| Functional Requirement | Non-Functional Requirement |
|---|---|
| User registration | Response time < 200 ms |
| User login | 99.9% availability |
| Product catalogue | Handle 1 million requests |
| Apply coupon | Encryption |
| Payment | Scalability |
| Order tracking | Fault tolerance |

A strong system design starts by clearly separating these two categories.

---

# 7. Important Non-Functional Requirements

## 7.1 Scalability

**Scalability** is the ability of a system to handle increasing workload by adding resources.

There are two major approaches.

### Vertical Scaling

Increase the resources of an existing machine.

```text
Before:

Server
4 CPU
8 GB RAM

        |
        v

After:

Server
16 CPU
64 GB RAM
```

Advantages:

- Simple
- Requires fewer architectural changes

Limitations:

- Hardware has an upper limit
- Can become expensive
- Does not eliminate the single-server failure problem

---

## 7.2 Horizontal Scaling

Add more machines instead of making one machine bigger.

```text
             Load Balancer
                  |
        +---------+---------+
        |         |         |
        v         v         v
     Server 1  Server 2  Server 3
```

Advantages:

- Better scalability
- Better fault tolerance
- Traffic can be distributed across multiple servers

Horizontal scaling is extremely important for large distributed systems.

---

# 8. Availability

**Availability** describes how often a system is operational and accessible to users.

It is commonly expressed as a percentage.

Examples:

- 99%
- 99.9%
- 99.99%
- 99.999%

The higher the availability requirement, the more carefully the architecture must handle failures.

### Availability and SLA

An organization may define an **SLA (Service Level Agreement)** specifying the expected availability and service performance.

For example:

```text
Availability target: 99.9%
```

A highly available system generally avoids relying on a single component that can bring down the entire application.

---

# 9. Reliability

**Reliability** is the ability of a system to continue operating correctly over time and under expected failure conditions.

A reliable system should protect against:

- Server failures
- Network failures
- Database failures
- Application crashes
- Hardware failures
- Data corruption

### Important Principle

> Reliability is not only about keeping the service running; it is also about maintaining correctness and preventing data loss.

---

# 10. Performance

Performance describes how efficiently a system processes requests.

Important performance metrics include:

- Latency
- Throughput
- Requests per second (RPS)
- CPU utilization
- Memory utilization
- Database query time

### Latency

Latency is the time taken to complete a request.

Example:

```text
User Request
     |
     v
Application
     |
     v
Database
     |
     v
Response

Total latency = 150 ms
```

A system may have a requirement such as:

```text
API response time < 200 ms
```

---

# 11. Security

Security protects the system and its data from unauthorized access and malicious activity.

Important security mechanisms include:

### Authentication

Determines **who the user is**.

Examples:

- Username/password
- OAuth
- JWT
- Multi-factor authentication

### Authorization

Determines **what the user is allowed to do**.

Example:

```text
Admin → Can delete users
User  → Cannot delete users
```

### Encryption

Protects sensitive information.

Encryption can be applied:

- In transit
- At rest

---

# 12. Maintainability

Maintainability describes how easily a system can be modified, tested, debugged, and extended.

A maintainable system generally uses:

- Modular architecture
- Clean code
- Clear API contracts
- Proper documentation
- Automated testing
- Separation of concerns
- Version control

For example:

```text
Authentication Module
Payment Module
Order Module
Notification Module
```

Instead of putting the entire application into one large codebase with tightly coupled responsibilities.

---

# 13. Observability

Observability is the ability to understand what is happening inside a system using its external outputs.

The three major pillars are:

### Logs

Record important events.

Example:

```text
2026-10-07 15:20:01
PaymentService
Payment failed for order 12345
```

### Metrics

Numerical measurements such as:

- CPU usage
- Memory usage
- Request rate
- Error rate
- Latency

### Traces

Track a request as it moves through multiple services.

```text
Client
  |
  v
API Gateway
  |
  v
Order Service
  |
  v
Payment Service
  |
  v
Database
```

Observability is essential for production systems because failures are inevitable.

---

# 14. Types of Applications

Applications can broadly be considered as:

1. Data-intensive applications
2. Compute-intensive applications

---

# 15. Data-Intensive Applications

A **data-intensive application** is one where the primary challenge is handling, storing, transferring, retrieving, or processing large amounts of data.

Examples:

- Instagram
- Banking systems
- E-commerce platforms
- Social media platforms
- Data analytics systems

### Example: Instagram

An Instagram-like application has to handle:

- Millions of users
- Large numbers of posts
- Images and videos
- Comments
- Likes
- Followers
- Feeds

The major challenge is often managing and moving large amounts of data efficiently.

### Important Questions

For a data-intensive application, ask:

1. How will we collect the data?
2. How will we store the data?
3. How will we retrieve the data?
4. How will we scale the data layer?
5. How will we protect the data?
6. How many users can access the system simultaneously?
7. What happens if a database fails?

---

# 16. Compute-Intensive Applications

A **compute-intensive application** is one where the major bottleneck is CPU/GPU computation rather than data movement.

Examples:

- Image processing
- Cryptography
- Machine learning
- Video processing
- Scientific simulations
- Rendering

For these systems, we focus heavily on:

- CPU utilization
- GPU utilization
- Parallel processing
- Algorithm efficiency
- Memory requirements
- Processing time

### Example

Suppose a system performs image processing.

```text
Input Image
     |
     v
Image Processing
     |
     v
Processed Image
```

If processing takes too long, we may need:

- Parallel processing
- GPU acceleration
- Better algorithms
- Distributed computation

---

# 17. Data-Intensive vs Compute-Intensive

| Data-Intensive | Compute-Intensive |
|---|---|
| Large data movement/storage | Heavy computation |
| Database is often critical | CPU/GPU is often critical |
| Caching is important | Parallelism is important |
| Replication/sharding may be required | GPU/CPU optimization may be required |
| Example: Instagram | Example: Image processing |
| Example: Banking | Example: ML training |

### Important Observation

A real-world system can be both data-intensive and compute-intensive.

For example, a machine learning platform may need:

- Large datasets
- Object storage
- Databases
- GPUs
- Distributed computation

---

# 18. Core Components of a System

A typical distributed application can contain the following components:

```text
                 Users
                   |
                   v
              Client App
                   |
                   v
             Load Balancer
                   |
          +--------+--------+
          |        |        |
          v        v        v
       Server   Server   Server
          |        |        |
          +--------+--------+
                   |
             Application
                   |
       +-----------+-----------+
       |           |           |
       v           v           v
   Database      Cache    Message Queue
```

The exact architecture depends on the requirements.

---

# 19. Database

The database stores persistent application data.

Two broad categories are:

### SQL Databases

Examples:

- PostgreSQL
- MySQL
- Oracle

They are commonly used when structured data and strong transactional guarantees are important.

### NoSQL Databases

Examples:

- MongoDB
- Cassandra
- DynamoDB

They are commonly considered when flexible schemas, massive scale, or specific access patterns make a NoSQL model appropriate.

---

# 20. Application Code

The application layer contains the business logic.

It commonly exposes APIs/endpoints.

Example:

```text
POST /users
POST /login
GET /products
POST /orders
GET /orders/{id}
```

The application layer communicates with databases, caches, queues, and other services through appropriate protocols.

---

# 21. Client Application

The client is the interface through which users interact with the system.

Examples:

- Web application
- Android application
- iOS application
- Desktop application

The client communicates with the backend through APIs.

```text
Client
  |
  | HTTP/HTTPS
  v
Backend API
```

---

# 22. Networking Protocols

Components of a distributed system need to communicate.

Common protocols include:

- HTTP
- HTTPS
- TCP
- UDP
- WebSocket
- gRPC

For example:

```text
Client
   |
 HTTPS
   |
   v
API Server
```

The choice of protocol depends on the communication requirements.

---

# 23. Cache

A cache stores frequently accessed data in a faster storage layer.

The basic idea is:

```text
Request
   |
   v
Cache
  / \
Hit  Miss
 |     |
 v     v
Data  Database
```

If data is available in the cache, the system can avoid an expensive database operation.

### Why use caching?

Caching can:

- Reduce database load
- Reduce latency
- Improve throughput
- Improve user experience

Common caching technologies include Redis and Memcached.

---

# 24. Load Balancer

A load balancer distributes incoming traffic among multiple servers.

```text
              Requests
                 |
                 v
           Load Balancer
          /      |      \
         v       v       v
      Server 1 Server 2 Server 3
```

### Why use a Load Balancer?

It helps with:

- Horizontal scaling
- Traffic distribution
- Availability
- Fault tolerance

If one server becomes unhealthy, the load balancer can stop sending traffic to it, depending on the configuration.

---

# 25. Message Queue

A message queue allows producers and consumers to communicate asynchronously.

```text
Producer
   |
   v
Message Queue
   |
   +------> Consumer 1
   |
   +------> Consumer 2
```

Queues are useful when work does not need to be completed synchronously within the user's request.

Examples:

- Sending emails
- Processing images
- Generating reports
- Order processing
- Background jobs

Common technologies include:

- Kafka
- RabbitMQ
- Amazon SQS

---

# 26. Database Scaling

As data and traffic increase, a single database may become a bottleneck.

Common database scaling techniques include:

### Replication

Create multiple copies of the database.

```text
             Primary DB
             /        \
            v          v
       Replica 1    Replica 2
```

Replication can improve:

- Read scalability
- Availability
- Disaster recovery capabilities

### Sharding

Split data across multiple database nodes.

```text
Users A-H  → Database 1
Users I-P  → Database 2
Users Q-Z  → Database 3
```

Sharding can allow a system to handle much larger datasets and workloads, but introduces additional complexity.

---

# 27. Consistency

When data is distributed across multiple systems, we need to think about consistency.

Consistency asks:

> Do different components observe the data in the expected state?

For example, suppose a user updates their profile.

```text
Application
     |
     v
Primary Database
     |
     v
Replica
```

If replication is asynchronous, the replica may temporarily contain the old value.

This introduces the concept of **eventual consistency**.

The correct consistency model depends on the application's requirements.

---

# 28. System Design Example: Banking / Cash Counter

Consider a bank where customers need to withdraw cash.

A simple system may have:

```text
Customer
   |
   v
Cash Counter
   |
   v
Application
   |
   v
Database
```

Now consider the following problems.

### Issue 1: Process is slow

The customer has to:

1. Understand the requirement
2. Enter the required information
3. Wait for the receipt
4. Receive the cash

Possible solution:

- Improve the application workflow
- Reduce unnecessary operations
- Automate repetitive work

---

### Issue 2: Increase in throughput

Suppose processing takes 5 minutes per customer.

If the process can be optimized to 3 minutes per customer, the number of customers served per hour increases.

Possible solutions:

- Improve the software workflow
- Automate operations
- Add additional counters
- Improve the underlying system

---

### Issue 3: Waiting Time

Suppose 10 customers are waiting and one counter takes 2 minutes per customer.

The waiting time can become significant.

Possible solution:

```text
                 Load Balancer
                /      |      \
               v       v       v
           Counter 1 Counter 2 Counter 3
```

This is analogous to horizontal scaling.

---

### Issue 4: Underutilized Counter

If some counters are overloaded while another is idle, requests should be distributed more intelligently.

A load-balancing mechanism can distribute work across available servers.

---

### Issue 5: Shared Database

Multiple application servers may need access to a common data source.

```text
Server 1 ----\
              \
Server 2 ------> Shared Database
              /
Server 3 ----/
```

This introduces important questions around:

- Database capacity
- Concurrent access
- Transactions
- Connection management
- Consistency

---

# 29. Example: Instagram Feed

Consider an Instagram-like feed.

The system needs to:

1. Fetch posts
2. Sort/rank posts
3. Show images/videos
4. Handle comments and likes
5. Store user relationships
6. Serve content to millions of users

A simplified architecture could be:

```text
                    Users
                      |
                      v
                 Client App
                      |
                      v
                Load Balancer
                      |
              +-------+-------+
              |       |       |
              v       v       v
           Server  Server  Server
              |
      +-------+--------+
      |       |        |
      v       v        v
    Cache  Database  Queue
              |
              v
         Object Storage
```

For a large-scale system, additional components could include:

- CDN
- Multiple databases
- Database replicas
- Message queues
- Feed generation services
- Recommendation services
- Monitoring infrastructure

---

# 30. CDN

A **Content Delivery Network (CDN)** stores and serves content from geographically distributed locations.

It is especially useful for:

- Images
- Videos
- JavaScript files
- CSS
- Static content

For example:

```text
User
 |
 v
CDN
 |
 +----> Nearby Edge Location
 |
 v
Origin Server
```

This reduces the distance between users and frequently requested content.

---

# 31. A Practical System Design Thinking Process

When approaching a system design problem, use the following sequence.

## Step 1: Clarify Requirements

Ask:

- What exactly are we building?
- Who are the users?
- What are the core features?
- What is out of scope?

---

## Step 2: Define Functional Requirements

Write down what the system must do.

Example:

```text
User can:
- Register
- Login
- Upload content
- View content
- Like content
- Comment on content
```

---

## Step 3: Define Non-Functional Requirements

Determine:

- Expected traffic
- Latency requirements
- Availability
- Scalability
- Security
- Reliability
- Data retention
- Consistency requirements

---

## Step 4: Estimate Scale

Estimate:

- Number of users
- Requests per second
- Data generated per day
- Storage requirements
- Read/write ratio

For example:

```text
Users = 10 million
Daily active users = 2 million
Requests/sec = 50,000
```

The exact architecture depends heavily on these numbers.

---

## Step 5: Identify Major Components

Start with a simple architecture:

```text
Client
   |
   v
API Server
   |
   v
Database
```

Then introduce additional components only when the requirements justify them.

---

## Step 6: Identify Bottlenecks

Ask:

- Is the application server overloaded?
- Is the database overloaded?
- Are reads too slow?
- Are writes too slow?
- Is network bandwidth a problem?
- Is some computation expensive?

---

## Step 7: Apply Scaling Techniques

Depending on the bottleneck, consider:

- Horizontal scaling
- Load balancing
- Caching
- Database replication
- Database sharding
- Message queues
- CDN
- Asynchronous processing

---

## Step 8: Design for Failure

Ask:

> What happens if this component fails?

For each important component, consider:

- Failure detection
- Retry
- Timeout
- Fallback
- Replication
- Recovery

---

## Step 9: Add Observability

Define:

- Logs
- Metrics
- Traces
- Alerts
- Health checks

A production system should not be designed only for the happy path.

---

# 32. Important System Design Questions

During system design, continuously ask:

### Functional

- What does the system need to do?
- What are the core APIs?
- What are the main user workflows?

### Scale

- How many users?
- How many requests per second?
- How much data?
- How fast will the data grow?

### Performance

- What is the expected latency?
- Which operations are expensive?
- What can be cached?

### Database

- SQL or NoSQL?
- What are the read/write patterns?
- Do we need replication?
- Do we need sharding?
- What consistency model is required?

### Reliability

- What happens if a server fails?
- What happens if the database fails?
- How do we recover?

### Availability

- What availability target is required?
- Do we need multiple instances?
- Do we need multiple availability zones/regions?

### Security

- How are users authenticated?
- How is authorization handled?
- Is sensitive data encrypted?

### Operations

- How will we monitor the system?
- How will we detect failures?
- How will we debug production problems?

---

# 33. Key Principle: Start Simple, Then Scale

One of the most important system design principles is:

> **Do not introduce distributed-system complexity before the requirements justify it.**

Start with:

```text
Client
  |
  v
Application
  |
  v
Database
```

Then evolve the architecture based on actual bottlenecks.

For example:

```text
Client
  |
  v
Load Balancer
  |
  +----> Application 1
  +----> Application 2
  +----> Application 3
             |
        +----+----+
        |         |
       Cache    Database
                  |
               Replica
```

Later, if asynchronous processing is required:

```text
Application
    |
    v
Message Queue
    |
    v
Workers
```

This approach keeps the architecture understandable while allowing it to evolve with scale.

---

# 34. Summary

System design is fundamentally about making engineering trade-offs while satisfying requirements.

The most important concepts introduced in this chapter are:

- Functional requirements
- Non-functional requirements
- Scalability
- Vertical scaling
- Horizontal scaling
- Availability
- Reliability
- Performance
- Security
- Maintainability
- Observability
- Data-intensive systems
- Compute-intensive systems
- Databases
- Caching
- Load balancing
- Message queues
- Replication
- Sharding
- Consistency
- CDN
- Fault tolerance

A strong system designer does not simply memorize components.

Instead, they learn to reason:

```text
Requirement
    ↓
Workload
    ↓
Bottleneck
    ↓
Architecture
    ↓
Trade-off
    ↓
Scalability + Reliability
    ↓
Production System
```

## Final Mental Model

When you see any system design problem, think:

> **What does the system need to do?**
>
> **How much traffic and data will it handle?**
>
> **What are the important non-functional requirements?**
>
> **Where will the bottleneck appear?**
>
> **What component solves that bottleneck?**
>
> **What happens when that component fails?**

That reasoning process is more important than memorizing any particular architecture.

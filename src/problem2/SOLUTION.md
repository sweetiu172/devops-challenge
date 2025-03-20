Provide your solution here:


# Requirements gathering
- RPS: 500 RPS
- Response time: p99 < 100ms
- resilient to failures
- high availability
- scalability
- cost-effective
- Key features: (based on binance)
    - Wallet management
    - Trading cryptos
    - Trading history management

# High level design
- There are 3 main features in this simple setup:
    - Wallet management: Users can view their wallet balance
    - Trading history management: Users can view their trading history
    - Trading cryptos: Main function of the system, users can place their orders to buy and sell cryptos
- 4 main flows in high level design:
    - Red one: View wallet balance, sell cryptos
    - Green one: View candlestick chart
    - Blue one: Buy cryptos
    - Black one: View trading history

![High level design](./High%20level%20design.png)

# Infrastructure design
## Components selected
- For scalability requirements, we can deploy the microservices system in either an ECS or EKS cluster. In this case, I would opt for EKS because it offers greater control over the cluster.
- The design also incorporates inbound and outbound queues, which enable decoupling and asynchronous processing. We can choose from AWS SQS, Apache Kafka, or AWS Kinesis Data Streams. For cost efficiency, I would select AWS SQS, with EKS pods communicating via a VPC endpoint.
- For the User DB and Trading History DB, I would use a relational database to ensure ACID compliance.
- Given the high traffic and streaming requirements of the Trading DB, we have the option of using either DynamoDB or Redis. I would choose Redis because it is an in-memory database, whereas DynamoDB relies on SSD storage.
- The frontend will be hosted in an S3 bucket and served through the CloudFront CDN.
- For global performance optimization, we can utilize AWS Global Accelerator to reduce latency.
- We will use AWS Route 53 to manage DNS.
- For Observability & Monitoring, I would use Cloudwatch and XRay

## High Availability & Scalability Strategy
- High Availability
    - Multi-AZ Deployments: All critical services (compute, database, cache) are deployed across multiple availability zones to ensure redundancy and fault tolerance.
    - Auto Scaling Groups: Compute instances (or container tasks) automatically scale based on demand, ensuring that the system can handle spikes (e.g., sudden trading surges) without degrading performance.
    - Load Balancing: ALB distributes incoming requests across multiple service instances to prevent overloading a single instance.

- Scalability
    - Stateless Microservices: Services are designed to be stateless, allowing horizontal scaling by simply adding more instances without complex session management.
    - Managed Services: Leveraging managed services (Aurora, ElastiCache, SQS) ensures that scaling of the data layer is handled by AWS, minimizing operational overhead.
    - Containerization (EKS): Containerized services can be easily updated and scaled out. Container orchestration enables rolling deployments and efficient resource utilization.
    - Asynchronous Processing: Using a messaging layer (SQS) allows decoupling heavy processing tasks from real-time order execution, improving system responsiveness.

## Final AWS Design
![AWS Design](./AWS%20Infra%20design.png)


# Future enhancements
- Resilient Event-Driven Architecture: As the system evolves, incorporate event streaming platforms (e.g., AWS MSK for Kafka) to efficiently manage larger data volumes and support more complex processing pipelines.
- Failover Cluster: Implement a secondary EKS cluster to ensure continuous availability during maintenance or unexpected downtime, leveraging Route 53 for seamless failover.
- Global Databases: Enable a Global Datastore for ElastiCache and deploy Aurora Global Database to enhance data replication and global availability.
- Enhanced Security: Strengthen security by implementing multiple network layers, including VPC Peering, to further isolate and protect critical resources.


# Reference document
https://drive.google.com/file/d/11bjXkzR9eQ8vZL4zqwxCN5qP9jyvisO2/view?usp=sharing



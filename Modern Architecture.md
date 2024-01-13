# Modern Architecture

## 1 - Application Scaling

### Scaling an Application
- **Benefits**: Increases system resilience and load capacity.

### Vertical Scaling
- **Description**: Enhancing capacity by upgrading the hardware of a single machine.

### Horizontal Scaling
- **Description**: Distributing workload across multiple machines, adding more to the cluster as needed.

### Application Clustering
- **Description**: Running multiple instances of the same application in parallel, managed by a load balancer.

### Stateless Application
- **Concept**: Design where nodes don't retain information from one request to another. External storage (like databases) is used for session data, enhancing failure recovery and update processes.

### High Availability
- **Measure**: Expressed as a system's uptime percentage over a year.
  - **Series Configuration**: `T = A * A`
  - **Parallel Configuration**: `T = 1 - (1 − A)²`

### Caching
- **Purpose**: Temporarily stores web content for faster access and efficiency. Implemented in HTTP servers, load balancers, client applications, and CDNs. Utilizes `Cache-Control: max-age` and headers like `ETags` or `Last-Modified`.

### Microservice Architecture
- **Design**: Divides a large application into small, independent units for easier management and development. Facilitates continuous delivery but can increase inter-service latency.

### REST API
- **Function**: Uses HTTP methods for interoperable service communication, exchanging data in formats like JSON/XML. Includes a URI, HTTP method, and media type, with responses containing a status code and payload.

### CRUD API
- **Operations**: Create (`POST`), Read (`GET`), Update (`PUT`), and Delete.

<br> 

## 2 - NoSQL Database

### General
- **NoSQL Databases**: Non-relational systems that prioritize availability and partition tolerance, sometimes at the cost of consistency.

### Types of NoSQL Databases
- **Key-Value Store**: Simple, efficient, allows arbitrary object storage (e.g., Redis).
- **Document Database**: Stores data in semi-structured formats like JSON/XML, reduces the need for joins (e.g., MongoDB).
- **Wide Column Database**: Stores data in tables with dynamic, row-specific columns (e.g., Cassandra).
- **Graph Database**: Models data as nodes and edges, ideal for complex relationships (e.g., Neo4j).

### Distributed Data Store
- **Concept**: Involves replicating data across machines, balancing high availability, and resilience with complex trade-offs.

### CAP Theorem
- **Principle**: In a distributed system, it's impossible to simultaneously achieve Consistency, Availability, and Partition Tolerance.

### ACID vs. BASE in Databases
- **ACID**: Ensures Atomicity, Consistency, Isolation, and Durability in traditional databases.
- **BASE**: Emphasizes Basic Availability, Soft-state, and Eventual consistency in distributed databases.

### Strategies for Scaling
- **Sharding**: Distributes data across nodes based on keys but can increase latency and complexity in cross-shard operations.
- **Optimistic Locking**: Avoids locking by checking data modifications at commit time, suitable for low-conflict scenarios.
- **Read/Write Quorum**: Ensures data consistency and reliability in distributed environments through node consensus.

### Example Queries
- **Document Database Query** (MongoDB/Couchbase):

```
{	"id" : 21340,
	"firstname" : "Joe",
	"lastname" : "Dalton",
	"addresses" : [
		{
			"line" : "31 cross road",
			"postcode": "63000",
			"city": "Document City",
			"country": "France"
		}
	]
}
```

- **Graph Database Query (Neoj4)** : 
```
MATCH (p:Person)
RETURN p
LIMIT 1
```

<br> 

## 3 - Resiliency Pattern

### Choosing the Correct Wire
- **Concept**: Selecting appropriate wire systems based on system load.
  - Use 32A wires for socket rings.
  - Use 6A wires for lighting.

### Use of Fuses
- **Purpose**: Protect circuits from overload by blowing when capacity is exceeded.
- **Implementation**: Sized according to the protected circuit and used ubiquitously.

### Apply Firestop to Electric Cable
- **Function**: Limits smoke spread and fire to a single room using fire-rated layers.
- **Design**: Known as Partition or Bulkhead pattern.

### Use of Circuit Breaker
- **Operation**: Compares incoming and outgoing electricity, breaking the circuit upon detecting discrepancies (e.g., when a human touches a wire).

### Energy Arrival to Houses
- **Components**: Electrical Grid consisting of Transport, Distribution, and Delivery Networks.

### Creation of Blackouts
- **Cause**: Cascading failures due to interconnected part failures.

### Thread Pool
- **Function**: Maintains multiple threads for concurrent task execution, commonly used in applications and databases.

### Resilience Tools
- **Example**: Resilience4j in Java, offering decorators for Circuit Breaker, Rate Limiter, Retry, or Bulkhead.

### Throttling
- **Concept**: Protects systems from overloading, similar to electrical fuses.
- **Implementation**: Sized according to maximum system load, implemented via connection pool sizing.
- **Formula**: `PoolSize = ResponseTimeSec * MaxLoad`

### Circuit Breaker Pattern
- **Function**: Detects and prevents failures, allowing for graceful service degradation and recovery time for failed services.

### RAID Storage
- **Use**: Data replication in servers for resilience.
- **Types**:
  	- **RAID0**: Combines multiple disks for greater throughput.
  	- **RAID1**: Data mirroring across disks.
  	- **RAID5**: Uses parity bits for data integrity, allowing for one disk failure without data loss.

### RAID Storage - Parity
- **Function**: Simple error detection by counting bits across disks to create parity bits, allowing disk data reconstruction.

### Align Timeout
- **Configuration**: Setting a timeout for external calls to avoid prolonged waiting and errors.

### Retry Pattern
- **Strategy**: Retrying failed service calls, with immediate retries for synchronous calls and exponential backoff for asynchronous or batch processes.

### Bulkhead Pattern
- **Inspiration**: Ship bulkheads, creating isolated compartments.
- **Application**: Compartmentalizes application parts to contain failures, using physical resource separation (e.g., separate VMs or containers).
- **Usage**: Provides quality service guarantees for specific user groups (e.g., paying vs non-paying customers).

### Queue-Based Load Leveling
- **Mechanism**: Uses queues for decoupling task generation from processing, leveling traffic spikes.
- **Considerations**: Ensuring the queue isn't a bottleneck and the worker service can handle average task loads.

### Bounded Result Set
- **Principle**: Limiting the amount of data processed or returned in queries, using pagination for REST APIs and database query optimizations.
- **Context**: Acceptable for unbounded sets in BI, Analytics, or batch systems with pre-production validation.

### Health Endpoint Monitoring
- **Purpose**: Monitors the health status of individual nodes in distributed architectures like clusters or microservices.

### Break the Glass
- **Concept**: Emergency, traceable production access for humans without permanent access rights.

### Chaos Engineering
- **Objective**: Tests a software system's resilience against infrastructure, network, and application failures.

<br> 

## 4 - Event Driven Architecture

### Introduction
- **Concept**: Utilizes events to decouple services in IT organizations via message brokers (queues).
- **Benefits**: Reduces point-to-point service communication complexity, supports asynchronous operations, and facilitates introduction of new consumers.

### Publisher/Subscriber
- **Implementation**: Can be through file systems or databases, with publishers writing and subscribers polling for data.
- **Message Brokers**: Specialized databases that support event delivery notifications.
- **Characteristics**: Centralized components offering durability and atomicity.
- **Popular Brokers**: ActiveMQ, RabbitMQ, IBM MQ, Azure Service Bus.

### Delivery Approaches
- **Load Balancing**: Distributes messages across subscribers, sending each message to only one subscriber.
- **Fan Out**: Sends each message to all subscribers.

### Delivery Approach - Retention
- **Broker's Role**: Ensures delivery to subscribers, tracking each message's delivery status.
- **Challenges**: Requires storage sizing consideration, especially in fan-out scenarios during subscriber downtime.

### Difference with Databases
- **Data Persistence**: Message brokers retain messages until distributed to all subscribers, typically resulting in a smaller working data set compared to databases.
- **Query Capabilities**: Brokers offer ordered, transient data views, unlike databases that provide various query options.

### Limitations and Drawbacks
- **Centralized Logic**: Avoids the Enterprise Service Bus antipattern where too much logic is centralized.
- **Broker's Unbounded Nature**: Challenges in synchronizing data across systems due to non-permanent event storage.
- **Consumer Failures**: Can pressure brokers to retain messages for later distribution.

### Log-Based Approach
- **Design**: Removes tracking delivery constraints, with consumers responsible for tracking their message processing status.
- **Efficiency**: Utilizes append-only file storage for high throughput.

### Kafka - Organization and Resiliency
- **Topics**: Organized similarly to database tables and partitioned across servers.
- **Message Distribution**: Based on message keys, akin to primary keys in databases.
- **Message Tracking**: Uses incremental offsets for consumer tracking and resumption.
- **Replication**: Recommended factor of 3 for resilience.

### Kafka - Cleanup Policy
- **Policies**: Delete (traditional age-based deletion) and compact (retains latest record version based on message ID).
- **Compact Policy**: Runs as a background activity, maintaining essential data while removing older versions.

### Source of Truth
- **Compaction Policy**: Enables Kafka to be a data synchronization tool and a source of truth for other systems.

### Avro Schema
- **Efficiency**: Converts verbose JSON into compact, binary formats.
- **Serialization Example (JSON)**:
  ```
  {
    "firstname": "Joe",
    "lastname": "Black",
    "age": 41
  }
```
- **Corresponding Schema** : 
```
{
	"type": "record",
	"name": "user",
	"fields" : 
	[
		{"name": "firstname", type": "long"},
		{"name": "lastname", "type": "string"},
		{"name": "age", "type": "int"}
	]
}
```

### Popular Serialization Formats
- **Examples**: Avro, Protobuf, MessagePack.

### Schema Registry
- **Function**: Defines the message structure for topics, ensuring interoperability between producers and consumers.
- **Benefits**: Enforces uniform message structure, enables more compact serialization, and controls schema evolution.

### Event Sourcing
- **Method**: Transmits only the changes (delta) from one state to another, promoting smaller payloads and immutability.
- **Challenges**: Requires maintaining state on the consumer side, leading to an unbounded dataset.

### Event Sourcing on Demand
- **Optimization**: Uses data snapshots for time-based data retrieval.
- **Application**: Suitable for databases specialized in snapshot-based processing.

### Stream Processing
- **Capabilities**: Offers real-time data query and analytics, transforming data streams.
- **Tools**: Kafka with KSql, Spark Stream, Azure Stream Analytics.

### Stream Processing - Joining and Windowing
- **Joining Streams**: Enriches data by integrating additional lookup information.
- **Windowing**: Processes streams in discrete time windows, useful for counting or aggregating infinite data streams.

<br> 

## 5 - Data Lake

### Introduction
- **Concept**: Addresses the spread and size of data across services by centralizing storage in a single store.

### Distributed File Storage
- **Approach**: Promotes distributed file storage with a structure similar to file systems.
- **Example**: Hadoop and its HDFS (Hadoop Distributed File Storage).

### Hadoop and HDFS
- **Background**: Inspired by the Google File System, consisting of a Computation Layer (Map Reduce engine) and a Storage Layer (HDFS).
- **HDFS Design**: Built for commodity hardware, handles data replication across nodes using NameNode and DataNode architecture.
- **Mechanics**: NameNode manages the filesystem namespace; DataNodes store actual data, split into chunks and replicated for failover.

### Distributed Computing
- **Strategy**: Distributes processing across nodes, moving computation close to data.
- **HDFS Parallel**: Job Tracker and Task Tracker parallel to NameNode and DataNode for processing.

### Map / Reduce
- **Function**: Scales computation for large data volumes. Map function works in parallel on data subsets; Reduce function aggregates results.

### Map Reduce / Benefits
- **Advantages**: More scalable, avoids network bandwidth limitations, and supports retry on small data subsets for failed computations.

### Apache Spark
- **Evolution**: Seen as an alternative to Map/Reduce, centered around Resilient Distributed Dataset (RDD) for interactive, multi-query processing.

### Star Schema
- **Data Organization**: Common method in Data Lakes, normalizing data into fact and dimension tables.

### Star Schema – In Real Life
- **Complexity**: Real-world implementations often involve additional branching, leading to a snowflake schema.

### Star Schema to Large Table
- **Performance Optimization**: Combining dimensions into a large table to simplify queries and improve performance.

### Data as File (CSV)
- **Format**: CSV is a simple and common format for data file storage.

### Avro for JSON Storage
- **Usage**: Provides row-oriented JSON serialization with pros like complex type representation and schema enforcement, but with some inefficiencies in partial queries.

### Apache Parquet
- **Type**: Column-oriented data storage format, offering compact and partitioned data storage ideal for large datasets.

### Delta Format
- **Function**: Adds a layer on top of Parquet to support updates, schema evolution, ACID properties, and time travel, but with a limited ecosystem.

### Eventual Consistency
- **Concept**: In distributed systems, if no updates are made to a data item, eventually all accesses to it will return the last updated value.

### Approach 1: Directory of File
- **Method**: Storing tables in multiple partitions, a simple approach with risks like partial reads and corrupt state on transaction failure.

### Metadata in the DataLake
- **Storage**: Keeping increment definitions directly in the DataLake, like Delta logs.

### Delta: Writing New Data
- **Mechanics**: New data is written but not referenced in the metadata, implementing MVCC and snapshot isolation.

### Delta: Transaction Log and Atomicity
- **Operation**: Relies on atomic put-if-absent operations in the filesystem. Ensures transaction serializability and atomicity.

### Delta: ACID Property
- **Attributes**:
  	- **Atomicity**: Using storage put-if-absent operations for atomic transactions.
  	- **Consistency**: One transaction at a time for valid state transitions.
  	- **Isolation**: Transactions are isolated until committed.
  	- **Durability**: Ensured by the data lake's storage system.

### Streaming Ingest and Consumption
- **Application**: Facilitates streaming data into Delta tables, supporting real-time use cases and operational BI.
- **Syntax Example**:
`events.writeStream.format("delta").outputMode("append").start("/delta/events")`

### Data Mesh Introduction
- **Concept**: An approach to address limitations of centralized data lake architectures, promoting decentralized, domain-driven design.

### Data Mesh Concept
- **Principles**:
	- **Domain Ownership**: Clear data boundaries and ownership.
	- **Data as a Product**: Long-lived teams with clear responsibilities.
	- **Self-serve Data Platform**: Autonomous data access.
	- **Federated Computational Governance**: Unified governance across diverse data lakes.

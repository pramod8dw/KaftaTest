# Apache Kafka: Introduction & Demo

## Overview
This document serves as a guide for presenting a demo and knowledge-sharing session on **Apache Kafka**. It covers what Kafka is, how it's used, its advantages, internal architecture, and how it compares to other messaging systems like Google Pub/Sub and RabbitMQ. It also explores Kafka’s critical role in **event-driven distributed architectures**.

---

## Table of Contents
1. [What is Apache Kafka?](#what-is-apache-kafka)
2. [Why Kafka in Event-Driven Architectures?](#why-kafka-in-event-driven-architectures)
3. [Use Cases](#use-cases)
4. [Advantages of Kafka](#advantages-of-kafka)
5. [Kafka Architecture](#kafka-architecture)
6. [Kafka vs Pub/Sub vs RabbitMQ](#kafka-vs-pubsub-vs-rabbitmq)
7. [Kafka Demo Overview](#kafka-demo-overview)
8. [Conclusion](#conclusion)

---

## What is Apache Kafka?
Apache Kafka is a **distributed event streaming platform** capable of handling trillions of events per day. Originally developed at LinkedIn and now part of the Apache Software Foundation, Kafka is designed for **high-throughput, fault-tolerant, real-time data streaming**.

**Key concepts:**
- **Producer**: Sends data (events/messages) to Kafka
- **Consumer**: Reads data from Kafka
- **Broker**: Kafka server that stores data and serves clients
- **Topic**: A category/feed name to which records are sent
- **Partition**: Unit of parallelism in topics

---

## Why Kafka in Event-Driven Architectures?

Event-driven architectures (EDA) rely on **asynchronous communication** between services through events. Kafka provides a durable, scalable, and performant event log that enables:

- **Microservices coordination**: Services publish and subscribe to events instead of invoking each other directly.
- **Audit trails**: Kafka retains the full history of events, supporting traceability and compliance.
- **Reprocessing and recovery**: Consumers can replay historical events by resetting their offsets.
- **Loose coupling**: Kafka decouples producers and consumers, allowing independent evolution.
- **Central source of truth**: Kafka acts as the backbone of the event-driven system.

---

## Use Cases
- Real-time analytics and monitoring
- Log aggregation and processing
- Stream processing
- Event sourcing
- Messaging and decoupling microservices
- ETL pipelines (Extract, Transform, Load)

---

## Advantages of Kafka
- **High throughput**: Can process millions of messages per second
- **Scalability**: Easily scalable horizontally by adding more brokers/partitions
- **Durability**: Messages can be persisted for configurable retention periods
- **Fault tolerance**: Replication across brokers ensures data reliability
- **Decoupling**: Allows producers and consumers to evolve independently
- **Backpressure handling**: Consumers can read at their own pace

---

## Kafka Architecture

### Core Components
- **Producer**: Pushes records to a topic
- **Consumer**: Subscribes to topics and processes records
- **Broker**: Handles storage and retrieval of messages
- **Zookeeper** *(deprecated)*: Used for metadata and leader election
- **Kafka Controller** (KRaft mode): Handles cluster metadata and management (ZooKeeper replacement)

### Internals
- **Topics** are divided into **partitions**
- Each partition is an **ordered, immutable, append-only commit log**
- Messages are written to disk and **replicated** across brokers
- Consumers maintain their **offsets**, enabling replayability
- Kafka guarantees **per-partition ordering**

### Message Flow
1. Producer sends record to a topic
2. Kafka writes the record to a partition (based on key or round-robin)
3. Broker stores it on disk and replicates to followers
4. Consumer reads the record from the topic partition

---

## Kafka vs Pub/Sub vs RabbitMQ

| Feature                | Apache Kafka                          | Google Pub/Sub                  | RabbitMQ                        |
|------------------------|----------------------------------------|----------------------------------|----------------------------------|
| Message Model         | Log-based (pull)                      | Message queue (push/pull)       | Message queue (push)            |
| Durability            | High (disk-based, configurable)       | High                            | Medium                          |
| Throughput            | Very High                             | High                            | Moderate                        |
| Latency               | Low                                   | Low                             | Low                             |
| Ordering Guarantees   | Per-partition ordering                | Optional (per-key)              | Per-queue                       |
| Replayability         | Yes (offset management)               | Limited (retention-based)       | No                              |
| Use Case Fit          | Event streaming, big data pipelines   | Cloud-native pub/sub messaging  | Traditional messaging, RPC      |

---

## Kafka Demo Overview

### Prerequisites
- Kafka (local installation or via Docker)
- Kafka CLI tools or GUI like Kafka UI / AKHQ
- Sample producer/consumer code (Java, Python, Node.js)

### Demo Flow
1. **Start Kafka broker**
2. **Create a topic**
3. **Produce messages** using CLI or code
4. **Consume messages** using CLI or code
5. **Simulate offset handling and consumer groups**
6. **Show partition distribution and load balancing**
7. **Demonstrate consumer failure and rebalance**
8. **Optional: Use `kafka-consumer-groups.sh` to reset offsets**

---


### Bonus: Kafka Streams Overview

Kafka Streams is a client library for building applications and microservices that process and analyze data stored in Kafka. It allows you to build real-time, scalable, and fault-tolerant stream processing applications using standard Java APIs.

**Key Features:**
- No separate cluster required (runs as part of your app)
- Supports stateless and stateful operations (e.g., map, filter, join, windowed aggregations)
- Fault-tolerant with state recovery
- Integrates with Kafka topics for both input and output

**Example (Java):**
```java
StreamsBuilder builder = new StreamsBuilder();
builder.stream("input-topic")
       .mapValues(value -> value.toString().toUpperCase())
       .to("output-topic");
KafkaStreams streams = new KafkaStreams(builder.build(), props);
streams.start();
```


---

## Conclusion

Apache Kafka is a powerful, distributed, event-streaming platform that excels at high-throughput, real-time data processing. Its architecture provides robustness, scalability, and flexibility, making it suitable for a wide range of data-driven applications.

Kafka is not just a messaging system — it's an **event backbone**. For teams adopting microservices, building data pipelines, or requiring reliable event delivery, Kafka offers a strong foundation that outperforms traditional messaging systems in durability, scalability, and reusability.

As systems move toward real-time, decoupled architectures, Kafka’s role continues to grow as the **central nervous system of modern distributed applications**.

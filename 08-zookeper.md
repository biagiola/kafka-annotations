Summary: Lecture 8 - Zookeeper
This lecture explains the historical and evolving role of Zookeeper in the Apache Kafka ecosystem. Zookeeper has been a fundamental dependency for Kafka since its inception, acting as a centralized service for managing metadata and coordinating the cluster. Its key responsibilities have included:

Maintaining the list of active Kafka brokers.

Assisting in leader election for partitions when a broker fails.

Sending notifications to brokers about cluster changes (e.g., new topics, broker failures).

Storing critical Kafka metadata.

A crucial piece of historical context is that older versions of Kafka (pre-0.10) stored consumer offsets in Zookeeper. However, for many years now, consumer offsets have been stored in the internal Kafka topic __consumer_offsets, and this is a common point of confusion that the lecture clarifies.

The central message of this lecture is that the industry is in a transition phase away from Zookeeper. Starting with Kafka 3.x, a new mode called KRaft (Kafka Raft metadata mode) allows Kafka to operate without Zookeeper by using an internal Raft consensus protocol for metadata management (as defined in KIP-500). The goal is for Zookeeper to be completely removed in a future Kafka 4.0 release.

Despite this transition, understanding Zookeeper remains important because most current production clusters still use it. The lecture provides practical advice for a modern software engineer:

For Cluster Management: It is still recommended to use Kafka with Zookeeper in production until KRaft is declared production-ready in a future release.

For Client Applications (The Golden Rule): As a developer, you should never connect your producers, consumers, or administrative clients directly to Zookeeper. All modern Kafka clients and command-line tools (since around Kafka 2.2) are designed to connect only to Kafka brokers (bootstrap servers). Using Zookeeper in client configuration is an outdated practice that will not work in a Zookeeper-less future.

The reasons for moving away from Zookeeper include improving security (Kafka's security features are more robust), simplifying architecture, and making the system easier to manage and configure.

Key Takeaways:
Zookeeper has been Kafka's traditional metadata manager but is being phased out.

KRaft is the new, Zookeeper-less mode for Kafka, available from version 3.x onwards.

Never use Zookeeper for client connections. Modern applications must connect only to Kafka bootstrap brokers. This is a non-negotiable best practice for a "great modern-day Kafka developer."

While Zookeeper is still present in many systems, its role is purely internal to the cluster, and application developers should abstract themselves away from it entirely.
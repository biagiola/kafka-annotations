Summary: Lecture 9 - Kafka KRaft
This lecture covers the future of Apache Kafka: KRaft mode (Kafka Raft Metadata mode), which eliminates the long-standing dependency on Apache Zookeeper. This transition, initiated by KIP-500, is a major architectural shift designed to address several limitations of the Zookeeper-based design.

The primary motivation for moving to KRaft is to overcome scaling limitations. While Zookeeper could become a bottleneck for clusters with over 100,000 partitions, the KRaft protocol enables Kafka to scale seamlessly to millions of partitions. Beyond scalability, KRaft offers significant operational benefits: it simplifies maintenance and setup, improves overall stability and recovery times, and provides a unified security model since administrators only need to manage Kafka itself, not a separate Zookeeper system.

From a practical standpoint, KRaft simplifies the architecture by merging the metadata management into the Kafka protocol itself. Instead of a separate Zookeeper quorum managing the brokers, a Quorum Controller (a set of brokers using the Raft consensus protocol) now handles these duties. One broker in the quorum acts as the active controller (leader), while others are on standby, leading to faster controller shutdown and recovery.

The lecture provides crucial version context:

KRaft was first implemented in Kafka 3.x.

It was declared production-ready starting with Kafka version 3.3.1 (KIP-833).

The upcoming Kafka 4.0 release will support only KRaft, completely removing Zookeeper support.

In essence, KRaft represents a more scalable, simpler, and higher-performance architecture for Kafka. It consolidates the entire system into a single, cohesive process, marking a significant evolution in the platform's design and paving the way for its next generation of growth.

Key Takeaways:
KRaft removes the Zookeeper dependency, allowing Kafka to scale to millions of partitions.

It offers major benefits: easier management, improved performance (faster shutdown/recovery), a single security model, and a simplified architecture.

The transition is complete: KRaft is production-ready since Kafka 3.3.1 and will be the only option in the upcoming Kafka 4.0.

The new architecture replaces the external Zookeeper ensemble with an internal Quorum Controller made of Kafka brokers using the Raft protocol.
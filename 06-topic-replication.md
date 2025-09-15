Summary: Lecture 6 - Topic Replication
This lecture covers the critical concept of replication, which is essential for building fault-tolerant Kafka clusters in production. While a replication factor of one is acceptable for development, production environments require a higher replication factor, typically two or three, to ensure data availability if a broker fails.

Replication means that for each partition, copies (called replicas) are maintained on multiple brokers. For example, a topic with two partitions and a replication factor of two will have each partition stored on two different brokers. This ensures that if one broker goes down, the partition's data is still available on another broker, preventing data loss and maintaining system availability.

A key concept within replication is the distinction between the Leader and the In-Sync Replicas (ISR). For any given partition, one broker is designated as the leader. All producer writes and, by default, all consumer reads for that partition must go through this leader broker. The other brokers that hold copies of the partition are followers; their job is to replicate the data from the leader as quickly as possible. Followers that are successfully keeping up with the leader are known as "in-sync replicas" (ISR). If a leader fails, one of the in-sync replicas will automatically be promoted to become the new leader.

The lecture also highlights an important evolution in Kafka's functionality. Starting from Kafka version 2.4, a feature called Follower Fetching (or Consumer Replica Fetching) was introduced. This allows consumers to read data from the closest replica of a partition, not just the leader. This can significantly improve read latency for consumers and reduce cross-data-center network costs in distributed cloud environments. However, it's important to note that producer writes still must always go to the leader.

Key Takeaways:
Replication Factor: Defines the number of copies of a partition stored across different brokers. A factor of N allows your cluster to tolerate the failure of N-1 brokers.

Leader: One broker is the leader for each partition. All producer writes and (in older versions) all consumer reads are handled by the leader.

In-Sync Replicas (ISR): Followers that are successfully replicating data from the leader. If the leader fails, a new leader is elected from the ISR list.

Follower Fetching (Kafka 2.4+): A newer feature that allows consumers to read from the closest replica (follower) instead of only the leader, improving latency and reducing costs. Producers continue to write only to the leader.
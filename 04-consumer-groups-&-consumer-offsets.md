Summary: Lecture 4 - Consumer Groups and Consumer Offsets
This lecture dives into the mechanisms that allow Kafka consumers to scale and process data reliably: Consumer Groups and Consumer Offsets.

A Consumer Group is a set of consumers that collaborate to consume all messages from one or more topics. The group is identified by a unique group.id property. The key rule is that each partition is consumed by exactly one consumer within the same group. This allows the consumption load to be distributed across all members of the group.

For example, a topic with five partitions can be consumed by a group with three consumers. The partitions will be distributed among them (e.g., Consumer 1 gets two partitions, Consumers 2 and 3 get one or two each). This ensures that all messages in the topic are processed without duplication within the group. A critical point is that you cannot have more active consumers in a group than there are partitions. Any additional consumers will remain idle and inactive until a partition becomes available.

Crucially, multiple consumer groups can read from the same topic independently. Each group processes the entire stream of messages at its own pace. This architecture enables different applications (like a location dashboard and a notification service in the trucks example) to operate on the same data stream simultaneously by each defining their own consumer group.

To track progress, Kafka uses Consumer Offsets. This is the mechanism that allows a consumer group to remember where it left off reading in each partition. When a consumer has processed a message, it commits its current offset back to a special, internal Kafka topic named __consumer_offsets. This commit can be done automatically at regular intervals or manually by the application.

Committing offsets is vital for reliability. If a consumer fails and restarts, it can simply read the last committed offset from the __consumer_offsets topic and resume processing from that point, ensuring no data is lost. However, the timing of these commits directly defines the delivery semantics of your application:

At-least-once (Default): Offsets are committed after the message is processed. If processing fails after the commit but before the result is persisted, the message will be processed again upon restart. This can lead to duplicates, so processing should be idempotent (handling the same message twice without negative effects).

At-most-once: Offsets are committed as soon as the message is received, before it is processed. If processing fails, the message is lost forever as it will not be read again.

Exactly-once: Achieved for Kafka-to-Kafka workflows using the Transactional API, which ensures offsets are committed and messages are produced atomically. For Kafka to an external system, it requires an idempotent consumer and careful design.

Key Takeaways:
A Consumer Group (group.id) allows a pool of consumers to share the work of reading from a topic's partitions. Each partition is read by only one consumer in the group.

You can have multiple consumer groups on a single topic, each operating independently and processing the full stream of data.

The Consumer Offset is a checkpoint stored in Kafka (__consumer_offsets) that records a consumer group's progress in each partition.

The timing of offset commits determines your application's delivery semantics: at-least-once, at-most-once, or exactly-once. Most applications use at-least-once with idempotent processing.
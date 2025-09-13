Summary: Lecture 1 - Topics, Partitions, and Offsets
This lecture introduces the fundamental building blocks of Apache Kafka, starting with the concept of a Topic. A topic is a named stream of data within a Kafka cluster, analogous to a table in a database but without any constraints or data validation. You can have as many topics as you need, such as logs, purchases, or trucks_gps, each identified by a unique name. Topics accept messages in any format, including JSON, Avro, text, or binary. The sequence of messages flowing through a topic is called a data stream, which is the core of Kafka's function as a data streaming platform. It's crucial to understand that data is not queried from topics; instead, data is written using Producers and read using Consumers.

A topic is divided into one or more Partitions. Partitions allow a topic to be scaled and distributed across multiple servers. For example, a topic can be made up of 3, 10, or even 100 partitions. Each partition is an ordered, immutable sequence of messages. This means that once a message is written to a partition, it can never be changed or deleted (though it may be removed later by a retention policy). The order of messages is guaranteed only within a single partition.

As messages are written to a partition, they are assigned a unique, sequential id number called an Offset. The offset starts at 0 and increments for every new message in that partition. The analogy used is perfect: if a topic is like a book, then a partition is a chapter in that book, and the offset is the page number within that chapter. It's vital to note that offsets are specific to each partition; offset 3 in partition 0 is a completely different message from offset 3 in partition 1. Offsets always increment and are never reused, even after old messages are deleted.

The lecture uses the example of a trucks_gps topic to illustrate these concepts. A fleet of trucks (Producers) sends its GPS location every 20 seconds to this topic. Multiple services (Consumers), like a real-time dashboard or a notification service, can then read from this same stream of data simultaneously, which is a key strength of Kafka.

Key Specifics to Remember:
Immutability: Data written to a partition is immutable; it cannot be changed or updated.

Retention: Data is not kept forever. By default, messages are deleted after one week, but this is configurable.

Ordering: Message order is guaranteed only within a partition, not across different partitions of the same topic.

Message Assignment: By default, messages are assigned to a random partition unless a key is provided, which we will explore in a future lecture.

Scalability: A topic can have as many partitions as needed, and determining the right number is an important design decision.


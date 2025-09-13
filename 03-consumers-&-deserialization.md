
Summary: Lecture 3 - Consumers and Deserialization
This lecture introduces the Consumer, the component that reads data from Kafka topics, completing the publish-subscribe model. Unlike a push model where the server sends data, Kafka consumers operate on a pull model. This means the consumer actively requests data from the Kafka brokers (servers), who then respond with the available messages.

A single consumer can read from one or multiple partitions. For example, one consumer might read from Partition 0, while a second consumer could read from both Partition 1 and Partition 2. Crucially, consumers are intelligent; they automatically know which broker to connect to in order to read from a specific partition and can seamlessly recover if a broker fails.

The most important guarantee a consumer provides is ordering within a partition. Data is always read from low to high offsets, ensuring the sequence of messages is preserved exactly as it was written. However, it's vital to remember that this ordering guarantee applies only within a single partition. There is no ordering across different partitions. A consumer reading from both Partition 1 and Partition 2 will get the correct order for each individual partition, but the interleaving of messages from different partitions is undefined.

When a consumer receives a message from Kafka, it receives the raw bytes that the producer originally sent. To make this data usable within an application, the consumer must reverse the process of serialization through Deserialization. The consumer must be configured with the correct Deserializers that match the Serializers used by the producer.

For instance, if the producer used an IntegerSerializer for the key and a StringSerializer for the value, the consumer must use an IntegerDeserializer and a StringDeserializer. This process transforms the binary key and value back into a usable integer object and string object, respectively. Kafka provides common deserializers for types like strings, integers, floats, Avro, and Protobuf.

This leads to a critical rule of Kafka topic design: consistency of data format. Once a topic is in use, the data types for the key and value must not change. If a producer were to suddenly switch from sending a string to sending an Avro record, all existing consumers would break because their deserializers would fail to interpret the new binary format. The correct way to evolve your data is to create a new topic for the new data format and migrate your producers and consumers to it, ensuring backward compatibility and preventing system failures.

Key Takeaways:
Consumers use a pull model to request data from Kafka brokers.

Message order is guaranteed only within a partition and is always from low to high offset.

Deserialization is the essential process of converting the bytes received from Kafka back into meaningful objects using the appropriate Deserializer.

Data Format Contract: There is a strict, implicit contract between producers and consumers. The data types used for keys and values must remain consistent for the entire lifespan of a topic. Changing data formats requires creating a new topic.
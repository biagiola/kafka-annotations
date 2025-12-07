Lecture 1:
. Why data is not query from the Topic directly, like we would do with db table in postgres, instead, for reading we need to use consumer, and for write, we need to use producer. We also need "something" in the middle, producer-consumer. Why is that?
. Offset are always increment and never reused? how can we understand this with a reald world example? E.g., truck gps system. And, even if old message are deleted, offset it is not reuse, so, are we going to have blank/empty spaces where messages were deleted?

Lecture 2
. We are mention "broker" before explain the concept.
. Who or how exactly which partition a message must be written to at the end? the producer, the kafka server or is in the payload specifying exactly where to put it?
. We set a key to avoid the default round-robin strategy, so message could go the desire same partition. But, we can achieve that using the kafka message property called "Partition and Offset"? what these kafka message differ each other?
. if we provide a key, we can ordering but we're loosing load balancing, hence, scalability ?
. why value property of a kafka message could be null? what's the benefits of that?

Lecture 3
. Can Producers update kafka messages?
. Do consumers read in sync mode?
. Can consumer read from multiple partitions in async mode?
. each consumer within a group reads from exclusive partitions. Why does it mean exclusive exactly? and how it can be related to inactive cosumers.

Lecture 4
. Why is said that kafka is only used as a transportation mechanism.?
. Murmur2 algorithm is part of the code logic of the Kafka partitioner, but what about round-robin?

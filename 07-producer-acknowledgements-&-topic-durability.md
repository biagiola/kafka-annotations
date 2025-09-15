Summary: Lecture 7 - Producer Acknowledgements and Topic Durability
This lecture introduces the crucial mechanism that producers use to guarantee message delivery: Acknowledgements (acks). This setting allows a producer to control the level of durability for its writes by determining how many brokers must confirm receipt of a message before it is considered successful.

There are three primary acks settings:

acks=0 (No Acknowledgement): The producer does not wait for any confirmation from the broker. This setting offers the highest throughput and lowest latency but provides no durability guarantee. If the broker fails immediately after receiving the message but before writing it, the data is lost.

acks=1 (Leader Acknowledgement - Default): The producer waits for an acknowledgement only from the leader broker of the partition. This confirms that the leader has written the message to its own log. This is a good balance between speed and durability, but limited data loss is still possible. If the leader acknowledges the write and then fails before the replicas have copied the message, that message

TODO: the producer always and only calls to the leader broker or could also call to a follower to make a writting operation?
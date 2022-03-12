# Theory

## Topics

- a particular stream of data
- similar to a table in database
- can have as many topics as we want
- defined by a name

## Partitions

- topics are split into partitions
- each partition is ordered
- each message within a partition gets an incremental id, called _offset_
- offsets only have meaning for a specific partition
- order is guaranteed only within a partition
- data is kept only for a limited time
- once the data is written to a partition, it can't be changed
- data is assigned randomly to a partition unless a key is provided

## Broker

- a Kafka cluster is composed of multiple brokers (servers)
- a broker is identified with its ID (integer)
- each broker contains certain topic partitions
- after connecting to any broker (called a bootstrap broker), you will be connected to the entire cluster
- topics should have a _replication factor_ > 1 (2 is okay, 3 is gold standard)
- at any time, only ONE broker can be a leader for a given partition and this leader can receive and service data for a partition. The other brokers will synchronize the data based on the replication factor.

## Producers

- producers write data to the topics
- producers automaatically know which broker and partition to write to
- in case of broker failures, producers will automatically recover
- producers can choose to receive acknowledgement of data writes (acks=0 -> won't wait for ack; acks=1 -> will wait for leader ack; acks=all -> leader + replicas ack)
- producers can choose to send a **key ** with the message. If key=null, then data is sent to the partitions in a round robin fashion. If key is sent, then all messages for that key will always go to the same partition

## Consumers

- consumers read data from topic(s)
- consumers know which broker to read from
- in case of broker failures, consumers know how to recover
- data is read in order within each partition

#### Consumer Groups

- consumers read data in consumer groups
- each consumer within a group reads form exclusive partitions
- if you have more consumers than partitions, some consumers will be inactive

#### Consumer Offsets

- Kafka stores the offsets at which a consumer group has been reading
- the offsets are committed live in a Kafka topic named \_\_consumer_offsets
- when a consumer in a group has processed data received from Kafka, it should be committing the offsets

#### Delivery Semantics for Consumers

- Consumers choose when to commit offsets
- The 3 delivery semantics are:
  1. **At most once**
     - offsets are committed as soon as the message is received
     - if the processing goes wrong, the message will be lost
  2. **At least once**
     - offsets are committed after the message is processed
     - if the processing goes wrong, the message will be read again
     - this can result in duplicate processing of messages. Make sure your processing is idempotent (i.e. processing the same messages won't impact the system)
  3. **Exactly once**
     - can be achieved for Kafka to Kafka workflows using Kafka Streams API
     - for Kafka to External Systems workflows, use an idempotnt consumer

## Zookeeper

- Zookeeper manages brokers (keeps a list of them)
- Zookeeper helps in performing leader election for partitions
- Zookeeper sends notifications to Kafka in case of changes
- Kafka can't work without Zookeeper
- Zookeeper by design operates with an odd number of servers (1, 3, 7...)
- Zookeeper has a leader (handles writes) and the rest of the servers are followers (handle reads)

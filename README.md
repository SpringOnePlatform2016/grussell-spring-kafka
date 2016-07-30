# grussell-spring-kafka
Spring for Apache Kafka @SpringOnePlatform

# Projects

## s1p-kafka

This project is used for the demos; it has a number of Spring Boot applications, demonstrating various capabilities of spring-kafka.

Each application is in package `org.s1p.appX` (where `appX` is app1, app2, etc).

Common configuration is in the `CommonConfiguration` class.

The apps require 2 topics to be created in Kafka...

`kafka-topics --zookeeper localhost:2181 --create --topic s1p.topic --partitions 5 --replication-factor 1`

`kafka-topics --zookeeper localhost:2181 --create --topic s1p.fooTopic --partitions 5 --replication-factor 1`

The first four apps use `String` serializers and deserializers for both the key and value.

### app1

This is the first app which simply sends a message to the `s1p.topic`.

### app2

This adds a simple `@KafkaListener` to receive messages from `s1p.topic` in a POJO method.

### app3

This shows how to access the partition from which the message was received in a `@KafkaListener` POJO method.

### app4

This shows a `@KafkaListener` where the topics/partitions are explicitly subscribed to, together with an initial offset.

### app5

This introduces JSON serializer and deserializer for the value.
It uses `JsonConfiguration` instead of `CommonConfiguration`; note that the type (`Foo`) is hard-coded into the deserializer - this is required since the JSON contains no type information.

### app6

This is a more flexible alternative to using the Json serializer and deserializer.
Instead, it uses `StringJsonMessageConverter` s and reverts to using String serializer/deserializer.
One benefit of this technique is the type used in the conversion is inferred from the method argument type.
Notice that we send a `Foo` and receive a `Bar` - of course, the types need to have compatible fields.

### app7

This builds on `app2` by adding retry and filtering to the listener.

### app8

This version of `app2` uses the lower-level `KafkaMessageListenerContainer` instead of a `@KafkaListener`.

### app9

This version of `app8` commits the offsets manually.

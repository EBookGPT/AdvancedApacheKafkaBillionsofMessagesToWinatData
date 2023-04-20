# Chapter 7: Conquer the Data Stream with Kafka Streams

Welcome back, my dear readers, to our ongoing adventure into the world of Apache Kafka! In the last chapter, we journeyed deep into Kafka Connect, learning the ins and outs of how to move data in and out of Kafka at scale. But what happens when we want to process that data in real-time? Enter Kafka Streams - the solution to all our real-time data processing needs!

In this chapter, we are thrilled to welcome a special guest, Neha Narkhede, co-founder of Confluent and one of the original creators of Apache Kafka! Together, we will dive deep into Kafka Streams and discover how it can help us process data in real-time, as well as delve into use cases and best practices for building powerful stream processing applications.

First, we'll start by exploring the fundamentals of Kafka Streams, from the notion of streams and tables to understanding the underlying topology of a Kafka Streams application. We'll then walk through some of the most common use cases for Kafka Streams, such as event-driven microservices, real-time analytics, and real-time ETL. Along the way, we'll provide real-life examples and code samples to help illustrate the concepts.

But that's not all! In addition to exploring the core concepts of Kafka Streams, we'll also dive into some of the more advanced features, including windowing, stateful processing, and interactive queries. We'll discuss the trade-offs involved in using these features and provide practical tips for getting the most out of your Kafka Streams applications.

So buckle up and get ready to conquer the data stream with Kafka Streams! Special guest Neha Narkhede will be with us every step of the way, sharing her insights and expertise as we explore this powerful stream processing framework. Let's get started!
# Story Time: Neha and the Robin Hood of Kafka Streams

Once upon a time, in the land of data processing, there lived a famous outlaw by the name of Robin Hood. Robin had become famous for his mastery of Apache Kafka, using it to distribute data and information to the people who needed it the most. His name had become synonymous with Kafka Connect, the framework he used to connect all the disparate systems and data sources across the land.

But as Robin looked out across the streams and rivers of the kingdom, he knew that there was still more that could be done. He dreamed of being able to process and analyze data in real-time, and he knew just the tool for the job - Kafka Streams.

Robin had heard rumors of a stream processing expert named Neha Narkhede, who had helped create Apache Kafka itself. Rumor had it that she was the only one who knew how to master the powerful Kafka Streams framework, and that she had the power to unlock its true potential.

Robin knew that he had to find her, so he set out on a journey through the dense forests and treacherous mountains, following the path to Neha's secret hideaway. After many days of travel, he finally arrived at her door, and was greeted with a warm welcome.

Over a hot cup of tea, Robin and Neha talked about their shared love of Kafka and how it had revolutionized data processing in the kingdom. Neha shared her knowledge and expertise of Kafka Streams, which left Robin in awe of the framework's capabilities.

Days turned into weeks, and the two became inseparable as they explored the power of Kafka Streams together. With Neha's guidance, Robin was able to turn his dream of real-time stream processing into a reality. Together, they used Kafka Streams to process data in real-time, building powerful applications that helped the people of the kingdom quickly and efficiently access the information they needed.

Thanks to Neha's expertise, Robin was able to master the art of Kafka Streams, becoming a legend among the people of the kingdom. Together, they had changed the game of data processing forever, with Robin earning the nickname "The Robin Hood of Kafka Streams".

And that, my dear readers, is how Robin Hood and Neha Narkhede conquered the data streams of the kingdom, using the power of Kafka Streams to win at data.
# Code Breakdown: Kafka Streams in Action

You've heard the story of Robin Hood and Neha Narkhede, who used their mastery of Kafka Streams to conquer the data streams of the kingdom. But what about the code that made it all possible? Let's take a closer look at some of the Kafka Streams code that was used to power their stream processing applications.

### 1. Building a Kafka Streams Topology

At the heart of every Kafka Streams application is its topology - a directed acyclic graph of processing nodes that represents the data flow of the application. Here's an example of how to build a simple topology in Kafka Streams, starting from a Kafka topic and ending with an output stream:

```java
final StreamsBuilder builder = new StreamsBuilder();

// create a Kafka stream from the input topic
KStream<String, String> inputStream = builder.stream("input-topic");

// transform the incoming data using a map function
KStream<String, String> transformedStream = inputStream
    .map((key, value) -> KeyValue.pair(key, value.toUpperCase()));

// write the transformed data to an output topic
transformedStream.to("output-topic");

final KafkaStreams streams = new KafkaStreams(builder.build(), config);
streams.start();
```

In this example, we use the `StreamsBuilder` class to create a new topology. We then create a stream using the `builder.stream()` method, which connects the topology to the "input-topic" Kafka topic.

Next, we apply a transformation to the stream using `map()`, which converts all incoming messages to uppercase. Finally, we write the transformed data to an output topic using `to()`. We then start the `KafkaStreams` application, which runs the entire topology.

### 2. Windowed Aggregation

In real-world use cases, we may need to aggregate data over time. Kafka Streams offers built-in support for windowed aggregations, making it easy to process data in a time-bound manner. Here's an example of how to perform windowed aggregation in Kafka Streams:

```java
final StreamsBuilder builder = new StreamsBuilder();

// create a Kafka stream from the input topic
KStream<String, String> inputStream = builder.stream("input-topic");

// create a tumbling window of 5 minutes
TimeWindowedKStream<String, String> windowedStream = inputStream
    .groupByKey()
    .windowedBy(TimeWindows.of(Duration.ofMinutes(5)));

// perform an aggregation on the windowed stream
KTable<Windowed<String>, Long> aggregatedStream = windowedStream
    .count(Materialized.as("store-name"));

// write the aggregated data to an output topic
aggregatedStream.toStream().to("output-topic", Produced.with(stringSerde, longSerde));

final KafkaStreams streams = new KafkaStreams(builder.build(), config);
streams.start();
```

In this example, we start by creating a Kafka stream from an input topic. We then group the stream by key and create a tumbling window of 5 minutes using `windowedBy()`. This allows us to perform aggregations on a time-bound basis.

Next, we perform a count aggregation on the windowed stream using `count()`, which returns a KTable with the aggregated data. Finally, we write the aggregated data to an output topic using `to()`.

### 3. Interactive Queries

Kafka Streams also offers support for interactive queries, which allow us to query the state of a Kafka Streams application in real-time. This can be useful for providing real-time feedback to users, or for debugging and monitoring purposes. Here's an example of how to leverage interactive queries in Kafka Streams:

```java
final StreamsBuilder builder = new StreamsBuilder();

// create a Kafka stream from the input topic
KStream<String, String> inputStream = builder.stream("input-topic");

// transform the incoming data using a map function
KStream<String, String> transformedStream = inputStream
    .map((key, value) -> KeyValue.pair(key, value.toUpperCase()));

// write the transformed data to an output topic
transformedStream.to("output-topic");

// create a state store for the transformed data
transformedStream.groupByKey()
    .count(Materialized.as("store-name"));

// build the topology and start the Kafka Streams application
final KafkaStreams streams = new KafkaStreams(builder.build(), config);
streams.start();

// query the state store using a Kafka Streams instance
ReadOnlyKeyValueStore<String, Long> keyValueStore = streams.store("store-name", QueryableStoreTypes.keyValueStore());
Long count = keyValueStore.get("some-key");
```

In this example, we create a Kafka stream and transform the incoming data using a `map()` function, as we did in the first example. We then create a state store for the transformed data using `groupByKey()` and `count()`, and give it a name using `Materialized.as()`.

Next, we build the topology and start the Kafka Streams application. Finally, we use a `KafkaStreams` instance to query the state store in real-time, using `store()` and `get()` to retrieve the current count for a given key.

These are just a few examples of the powerful stream processing capabilities offered by Kafka Streams. With the right knowledge and expertise, you too can conquer the data streams of the kingdom and become a legend like Robin Hood and Neha Narkhede!


[Next Chapter](08_Chapter08.md)
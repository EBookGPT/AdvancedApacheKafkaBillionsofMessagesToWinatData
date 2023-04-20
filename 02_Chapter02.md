# Chapter 2: Kafka Architecture and its Components

Welcome, dear readers, to the second chapter of our thrilling adventure into the world of Advanced Apache Kafka: Billions of Messages To Win at Data! In the last chapter, we embarked on our journey with an introduction to Apache Kafka. Now, it's time to dive deeper into the architecture of Kafka and understand its components.

But before we begin, we have a very special guest joining us in this chapter. She is the co-founder and CTO of Confluent, Neha Narkhede. Neha is also one of the co-founders of Apache Kafka and has been instrumental in designing and building Kafka's architecture. It's an honor for us to have her insights on the topic and we can't wait to learn from her.

So, let's get started! In this chapter, we will explore the inner workings of Kafka's architecture and its components. We will delve into Kafka's distributed system and learn about its core elements like producers, consumers, and brokers. We will also understand the role of ZooKeeper in managing Kafka's cluster and ensuring fault tolerance. 

Moreover, we will dive into the details of Kafka's messaging system, covering the topics of Kafka Topics, Partitions, and Offsets. We will explain how Kafka stores messages, and how they are partitioned and replicated across brokers. 

As seasoned adventurers in the world of advanced Kafka, we know that this journey won't be easy. There are countless obstacles that we may encounter, but we are confident that with the right tools at our disposal, we can overcome them all. And that's where the code samples come in! Our journey will be filled with thrilling code samples that will enable us to apply what we learn along the way. 

So, buckle up and get ready, dear readers. With Neha Narkhede by our side, we are about to embark on a magical journey that will take us to the heart of Kafka's architecture and its components. Let's code our way to victory!
# Chapter 2: Kafka Architecture and its Components - The Robin Hood Story

Once upon a time, in the kingdom of DataLand, there lived a hero named Robin Hood. Robin was a master archer, and his arrows always hit their mark. Robin was always in pursuit of new adventures, and his daring spirit led him to the land of Kafka.

Robin had heard about Kafka's advanced messaging system, and he was determined to master it. But the road ahead was perilous, and he needed someone who had been there before to guide him. That's when he met a special guest, the co-founder and CTO of Confluent, Neha Narkhede.

Neha was a wise and experienced Kafka adventurer, and she had designed and built Kafka's architecture herself. Robin was overjoyed to have Neha by his side and he knew that with her guidance, he could overcome any obstacle that lay ahead.

Their adventure began in the Kafka cluster, a distributed system comprising producers, consumers, and brokers. Neha showed Robin how brokers acted as message stowaways, storing incoming messages and forwarding them to the appropriate receivers.

But just when Robin thought he understood Kafka's architecture, he was faced with a new challenge. The kingdom of DataLand was geographically vast, and messages needed to be delivered quickly to their intended recipients.

Neha introduced Robin to Kafka's partitions and offsets. She explained how Kafka stored messages, breaking them up into smaller bite-sized pieces for quick delivery across the kingdom's vast landscape. Robin was impressed and thanked Neha for showing him the way.

But their journey was not over yet. In the depths of the Kafka cluster lay a challenge that would test even Robin's archery skills. Neha showed him how Kafka used ZooKeeper to manage the cluster and keep it fault-tolerant. Robin was amazed as Neha demonstrated how ZooKeeper acted as a watchful guardian, making sure that Kafka's brokers were always accessible and distributing load evenly across the cluster.

As Robin and Neha emerged from the Kafka cluster together, victorious, Robin knew that he had become a master of Kafka's architecture and its components. He had learned the vital principles of Kafka's distributed system, and he had gained a wise mentor in Neha Narkhede.

Dear readers, let Robin Hood and Neha Narkhede's Kafka journey be an inspiration to us all. The road ahead may be fraught with danger, but as long as we have the right tools and a wise guide at our side, we can overcome any obstacle and emerge victorious.
# Explanation of code used to resolve the Robin Hood story

Dear readers, in our Robin Hood story of mastering Kafka's architecture, we explored the core components of Kafka's distributed system, such as producers, consumers, brokers, and ZooKeeper. We also covered Kafka's messaging system, including its topics, partitions, and offsets. In this section, we will explain the code that was used to resolve the challenges faced in the story.

Our journey began with Kafka's brokers, which are responsible for storing and forwarding messages. In the code, we created a producer that sends messages to Kafka brokers using the Confluent Kafka Python library. Here is an example of the producer code:

```python
from confluent_kafka import Producer

producer = Producer({'bootstrap.servers': 'localhost:9092'})

def delivery_report(err, msg):
    if err is not None:
        print('Message delivery failed: {}'.format(err))
    else:
        print('Message delivered to {} [{}]'.format(msg.topic(), msg.partition()))

producer.produce('example_topic', key='key', value='value', callback=delivery_report)

producer.flush()
```

Next, we encountered the challenge of delivering messages quickly across a geographically vast landscape. Kafka's partitions and offsets allow for the efficient delivery of messages across the network. In our code, we created a consumer that can consume messages from a Kafka topic using Confluent Kafka Python library. Here is an example of the consumer code:

```python
from confluent_kafka import Consumer, KafkaError

consumer = Consumer({'bootstrap.servers': 'localhost:9092', 'group.id': 'mygroup', 'auto.offset.reset': 'earliest'})

consumer.subscribe(['example_topic'])

while True:
    msg = consumer.poll(1.0)

    if msg is None:
        continue
    if msg.error():
        if msg.error().code() == KafkaError._PARTITION_EOF:
            print('End of partition reached {}/{}'.format(msg.topic(), msg.partition()))
        else:
            print('Error while consuming message: {}'.format(msg.error()))
    else:
        print('Received message: {}'.format(msg.value()))

consumer.close()
```

Lastly, we encountered the challenge of ensuring fault tolerance in Kafka's architecture. All Kafka brokers coordinate with ZooKeeper, which manages the cluster topology and provides distributed synchronization. We created a ZooKeeper instance using the Kazoo library in Python. Here is an example of how to create a ZooKeeper client:

```python
from kazoo.client import KazooClient

zk = KazooClient(hosts='127.0.0.1:2181')

zk.start()

if not zk.exists('/example'):
    zk.create('/example')

children = zk.get_children('/')
print('ZooKeeper children:', children)

data, stat = zk.get('/example')
print('ZooKeeper data:', data)

zk.stop()
```

Dear readers, with these code snippets, you are now equipped to explore Kafka's distributed system and messaging system. As we learned in our Robin Hood story, Kafka can be a powerful tool in the world of data. Armed with the right skills, tools, and guidance, you too can become a master of Kafka's architecture and its components.


[Next Chapter](03_Chapter03.md)
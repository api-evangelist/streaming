---
title: API Design
url: https://kafka.apache.org/0100/implementation/api-design/
date: '0001-01-01'
author: ''
feed_url: https://kafka.apache.org/index.xml
---
Producer APIs The Producer API that wraps the 2 low-level producers - kafka.producer.SyncProducer and kafka.producer.async.AsyncProducer.
class Producer { /* Sends the data, partitioned by key to the topic using either the */ /* synchronous or the asynchronous producer */ public void send(kafka.javaapi.producer.ProducerData<K,V> producerData); /* Sends a list of data, partitioned by key to the topic using either */ /* the synchronous or the asynchronous producer */ public void send(java.util.List<kafka.javaapi.producer.ProducerData<K,V>> producerData); /* Closes the producer and cleans up */ public void close(); } The goal is to expose all the producer functionality through a single API to the client.

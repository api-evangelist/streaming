# Streaming

A topic index of streaming protocols, platforms, and processing engines tracked by API Evangelist.

This repository is the landscape map for **streaming** as a distinct discipline from `events` (discrete event envelopes) and `async-apis` (the AsyncAPI specification). Streaming is the real-time, high-volume, often bidirectional pipe between producers and consumers — partitioned, replayable, ordered (per-partition), and increasingly the spine of operational AI, observability, and customer-facing real-time experiences.

## Streaming vs Events vs Async APIs

| | Streaming | Events | Async APIs |
|---|---|---|---|
| Primary artifact | A partitioned, replayable log / pipe | A discrete event envelope (CloudEvents) | A contract document (AsyncAPI spec) |
| Ordering | Per-partition / per-key | Per producer | Per channel |
| Volume | High-throughput, sustained | Bursty, business-significant | Defined by the contract |
| Replay | Yes (offsets, sequence numbers) | Sometimes (event store) | Out of scope |
| Bidirectional | Often (WebSocket, gRPC bidi) | No | Defined per channel |
| Examples here | Kafka, Kinesis, WebSocket, Flink | CloudEvents, Webhooks | AsyncAPI 3.0 |

The three topics overlap (CloudEvents flow over Kafka, AsyncAPI describes Kafka channels), but the *primary* operational concerns differ enough that they get separate catalogs.

## The Streaming Landscape

### 1. Log-structured platforms

The reference architecture: an append-only, partitioned log with consumer-tracked offsets. Records are durable, replayable, and ordered within a partition.

- **[Apache Kafka](https://kafka.apache.org)** — the reference open-source streaming platform; topics, partitions, consumer groups, exactly-once, KRaft consensus.
- **[Apache Pulsar](https://pulsar.apache.org)** — cloud-native, multi-tenant, with BookKeeper tiered storage and native geo-replication.
- **[Redpanda](https://redpanda.com)** — Kafka-API-compatible, written in C++, single binary, no ZooKeeper/JVM.
- **[NATS JetStream](https://docs.nats.io/nats-concepts/jetstream)** — JetStream brings persistent streaming to NATS for edge, IoT, and microservice topologies.

### 2. Managed cloud streams

The hyperscalers offer fully managed log-structured or pub/sub services, often with Kafka-protocol compatibility.

- **[Amazon Kinesis](https://aws.amazon.com/kinesis/)** — Data Streams (shards), Firehose (delivery), Video Streams (media), HTTP/2 SubscribeToShard.
- **[Google Cloud Pub/Sub + Dataflow](https://cloud.google.com/pubsub)** — managed pub/sub with at-least-once and exactly-once, fed into Beam pipelines on Dataflow.
- **[Azure Event Hubs](https://learn.microsoft.com/en-us/azure/event-hubs/)** — Kafka-protocol compatible with Capture to ADLS / Blob.
- **[Confluent Cloud](https://www.confluent.io/confluent-cloud/)** — managed Kafka by Kafka's original authors, plus ksqlDB, Schema Registry, Stream Governance, and Flink.
- **[StreamNative](https://streamnative.io)** — managed Apache Pulsar from Pulsar's contributors.

### 3. Streaming protocols (over the wire)

The protocols an API consumer actually speaks when subscribing to a real-time feed. These are what end-users see in an SDK — the platform tier above is what an operator runs.

- **[Server-Sent Events (SSE)](https://html.spec.whatwg.org/multipage/server-sent-events.html)** — one-way HTTP streaming using `text/event-stream`; the de facto delivery for LLM token streams and live dashboards.
- **[WebSocket](https://datatracker.ietf.org/doc/html/rfc6455)** — full-duplex, bidirectional, RFC 6455; foundation for chat, collaborative apps, market data, and real-time control planes.
- **[gRPC streaming](https://grpc.io)** — server-streaming, client-streaming, and bidirectional RPC over HTTP/2; the default for service-to-service streaming and Kubernetes-native APIs.
- **[GraphQL Subscriptions](https://spec.graphql.org/draft/#sec-Subscription)** — GraphQL operation type for typed streams, typically over WebSocket via `graphql-ws`.

### 4. Connect frameworks and change data capture (CDC)

Streams aren't useful unless data flows into and out of them. The connector tier ingests from databases, file systems, and SaaS APIs and delivers to warehouses, search indexes, and lakes.

- **[Kafka Connect](https://kafka.apache.org/documentation/#connect)** — source/sink connector framework; distributed mode is a REST-controlled cluster of workers.
- **[Debezium](https://debezium.io)** — change-data-capture for Postgres, MySQL, MongoDB, SQL Server, Oracle, Cassandra; reads each database's native replication log and emits row-level change events into Kafka.

### 5. Stream processing

Once you have a stream, you process it: filter, enrich, join, window, materialize. This tier defines the event-time / watermark / window / trigger semantics that make stream processing correct.

- **[Apache Flink](https://flink.apache.org)** — distributed stateful stream processing; SQL, DataStream, Table APIs; reference engine for event-time, watermarks, and exactly-once state.
- **[Spark Structured Streaming](https://spark.apache.org/streaming/)** — micro-batch (and experimental continuous) processing on Spark SQL; "stream as unbounded table."
- **[Materialize](https://materialize.com)** — operational data warehouse maintaining incrementally updated materialized views via Differential Dataflow.
- **[Tinybird](https://www.tinybird.co)** — real-time analytics on ClickHouse, exposing SQL pipes as parameterized HTTP API endpoints.
- **[Bytewax](https://bytewax.io)** — Python-native stream processing on Timely Dataflow; built for data scientists and ML teams.
- **[Apache Beam](https://beam.apache.org)** — unified batch + streaming model that compiles to Dataflow, Flink, Spark, and Samza; defines the canonical streaming semantics.

## What's in this Repository

| File | Purpose |
|---|---|
| [`apis.yml`](apis.yml) | APIs.json catalog of every platform, protocol, and engine indexed here, with links to docs and topic-level API Evangelist repos. |
| [`vocabulary/streaming-vocabulary.yml`](vocabulary/streaming-vocabulary.yml) | Domain vocabulary — stream, topic, partition, offset, watermark, window, exactly-once, CDC, schema registry, and the rest. |
| [`json-schema/streaming-stream-schema.json`](json-schema/streaming-stream-schema.json) | JSON Schema for a `Stream` — a topic / shard / partitioned log as it appears in a catalog. |
| [`json-schema/streaming-stream-record-schema.json`](json-schema/streaming-stream-record-schema.json) | JSON Schema for a `StreamRecord` — the unit of data on a stream, generalized across Kafka, Pulsar, Kinesis, Pub/Sub. |
| [`json-schema/streaming-stream-platform-schema.json`](json-schema/streaming-stream-platform-schema.json) | JSON Schema for a `StreamPlatform` — catalog record for a Kafka, Pulsar, Kinesis, etc. installation. |
| [`json-structure/streaming-stream-structure.json`](json-structure/streaming-stream-structure.json) | JSON Structure description of a Stream record for control-plane catalog APIs. |
| [`json-ld/streaming-context.jsonld`](json-ld/streaming-context.jsonld) | JSON-LD context mapping the streaming vocabulary into schema.org-compatible linked data. |
| [`examples/`](examples/) | Reference payloads for a Stream, a Stream Record, and a Stream Platform record. |

## Related API Evangelist topic repos

- [`events`](https://github.com/api-evangelist/events) — discrete event envelopes, CloudEvents, webhook patterns.
- [`async-apis`](https://github.com/api-evangelist/async-apis) — AsyncAPI specification and async API contracts.
- [`asyncapi`](https://github.com/api-evangelist/asyncapi) — the AsyncAPI Initiative itself.
- [`webhooks`](https://github.com/api-evangelist/webhooks) — server-to-server callback patterns.
- [`cloudevents`](https://github.com/api-evangelist/cloudevents) — the CloudEvents specification.
- [`kafka`](https://github.com/api-evangelist/kafka) / [`apache-kafka`](https://github.com/api-evangelist/apache-kafka) — the Kafka project.
- [`apache-flink`](https://github.com/api-evangelist/apache-flink), [`materialize`](https://github.com/api-evangelist/materialize), [`amazon-kinesis`](https://github.com/api-evangelist/amazon-kinesis), [`kafka-connect`](https://github.com/api-evangelist/kafka-connect) — provider-level repos for individual streaming products.

---

Maintained by [Kin Lane](https://apievangelist.com) and the API Evangelist Network. Contributions and corrections welcome via PR.

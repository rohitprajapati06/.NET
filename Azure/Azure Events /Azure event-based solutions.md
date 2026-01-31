# Develop Event-Based Solutions (AZ-204)

> **Purpose**: This document is written for **Azure Developer Associate (AZ-204)** exam preparation.
> It is **GitHub-ready** and covers **Azure Event Grid** and **Azure Event Hubs** with clear explanations, diagrams, and code samples.

---

## 1. Event-Based Architecture Overview

Event-based architecture enables systems to react to changes asynchronously. Producers emit events, and consumers subscribe and react without tight coupling.

### Benefits

* Loose coupling
* High scalability
* Fault tolerance
* Real-time responsiveness

### Azure Services for Event-Based Solutions

| Service          | Purpose                                   |
| ---------------- | ----------------------------------------- |
| Azure Event Grid | Event notifications & reactive automation |
| Azure Event Hubs | High-throughput event streaming           |

---

## 2. Azure Event Grid

![Azure Event Grid Architecture](https://learn.microsoft.com/azure/event-grid/media/overview/event-grid-functional-model.png)

### What is Azure Event Grid?

Azure Event Grid is a **fully managed, push-based event routing service** used for **event notifications**.

It uses a **publish–subscribe (pub/sub)** model:

* Publishers emit events
* Event Grid routes them
* Subscribers handle them

> ⚠️ Event Grid **does not store events**.

---

### When to Use Event Grid (EXAM FAVORITE)

Use Event Grid when:

* You need **reactive workflows**
* Events are **lightweight**
* Multiple consumers must react
* Near real-time processing is required

**Examples**

* Blob uploaded → trigger Azure Function
* Resource created → trigger Logic App
* User registered → send email

---

### Core Concepts

| Concept            | Description                    |
| ------------------ | ------------------------------ |
| Event Source       | Service that emits events      |
| Topic              | Endpoint where events are sent |
| Event Subscription | Routing configuration          |
| Event Handler      | Service that processes events  |

---

### Supported Event Handlers

* Azure Functions ⭐
* Logic Apps
* Webhooks
* Azure Automation
* Service Bus Queue / Topic

---

### Event Schemas

| Schema            | Description          |
| ----------------- | -------------------- |
| Event Grid Schema | Default Azure schema |
| CloudEvents 1.0   | CNCF standard        |
| Custom Schema     | User-defined         |

---

### Publishing Custom Events (C#)

```csharp
var credentials = new AzureKeyCredential("<key>");
var client = new EventGridPublisherClient(
    new Uri("<topic-endpoint>"),
    credentials);

var events = new List<EventGridEvent>
{
    new EventGridEvent(
        "UserCreated",
        "MyApp.Users",
        "1.0",
        new { UserId = 101, Email = "test@test.com" })
};

await client.SendEventsAsync(events);
```

---

### Key Exam Notes – Event Grid

* Push-based delivery
* No event persistence
* Automatic retries (up to 24 hours)
* At-least-once delivery
* Best for **notifications**, not streaming

---

## 3. Azure Event Hubs

![Azure Event Hubs Architecture](https://learn.microsoft.com/azure/event-hubs/media/event-hubs-about/event-hubs-architecture.png)

### What is Azure Event Hubs?

Azure Event Hubs is a **big data streaming platform** designed to ingest **millions of events per second**.

Think of it as:

> **Kafka-like event ingestion for Azure**

---

### When to Use Event Hubs (EXAM FAVORITE)

Use Event Hubs when:

* High-volume telemetry
* Continuous event streams
* Multiple consumers reading independently
* Event replay is required

**Examples**

* IoT telemetry
* Application logs
* Clickstream analytics
* Real-time monitoring

---

### Core Concepts

| Concept          | Description                |
| ---------------- | -------------------------- |
| Event Producer   | Sends events               |
| Event Hub        | Stream container           |
| Partition        | Ordered sequence of events |
| Consumer Group   | Independent reader view    |
| Offset           | Position in the stream     |
| Throughput Units | Capacity measurement       |

---

### Partitioning (VERY IMPORTANT)

* Ordering is guaranteed **within a partition**
* Ordering is **not guaranteed across partitions**
* Partition key ensures same partition routing

---

### Retention & Replay

| Feature   | Event Hubs    |
| --------- | ------------- |
| Retention | 1–90 days     |
| Replay    | Yes           |
| Ordering  | Per partition |
| Storage   | Temporary     |

---

### Sending Events (C#)

```csharp
await using var producer = new EventHubProducerClient(
    "<connection-string>",
    "<event-hub-name>");

using EventDataBatch batch = await producer.CreateBatchAsync();
batch.TryAdd(new EventData(
    Encoding.UTF8.GetBytes("Telemetry data")));

await producer.SendAsync(batch);
```

---

### Consuming Events (C#)

```csharp
var consumer = new EventHubConsumerClient(
    EventHubConsumerClient.DefaultConsumerGroupName,
    "<connection-string>",
    "<event-hub-name>");

await foreach (PartitionEvent ev in consumer.ReadEventsAsync())
{
    Console.WriteLine(
        Encoding.UTF8.GetString(ev.Data.Body.ToArray()));
}
```

---

### Key Exam Notes – Event Hubs

* Pull-based consumption
* Extremely high throughput
* Supports replay
* Partition-based ordering
* Used for **streaming**, not notifications

---

## 4. Event Grid vs Event Hubs (EXAM COMPARISON)

| Feature    | Event Grid | Event Hubs      |
| ---------- | ---------- | --------------- |
| Event Type | Discrete   | Continuous      |
| Volume     | Low–Medium | Very High       |
| Retention  | ❌ No       | ✅ Yes           |
| Replay     | ❌ No       | ✅ Yes           |
| Ordering   | ❌ No       | ✅ Per partition |
| Delivery   | Push       | Pull            |

---

## 5. AZ-204 Decision Matrix

| Scenario                        | Recommended Service |
| ------------------------------- | ------------------- |
| Blob created → trigger Function | Event Grid          |
| High-volume telemetry ingestion | Event Hubs          |
| Multiple consumers with replay  | Event Hubs          |
| Serverless automation           | Event Grid          |
| Streaming analytics             | Event Hubs          |

---

## ✅ Summary

| Service          | Best Use Case                            |
| ---------------- | ---------------------------------------- |
| Azure Event Grid | Event notifications & reactive workflows |
| Azure Event Hubs | High-volume streaming & ingestion        |

---

📌 **Tip for AZ-204**: If the question mentions **notifications or triggers**, choose **Event Grid**. If it mentions **telemetry, logs, replay, or streaming**, choose **Event Hubs**.

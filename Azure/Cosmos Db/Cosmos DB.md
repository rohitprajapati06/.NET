# Develop Solutions that Use Azure Cosmos DB (AZ-204)

> **Purpose**  
> This document is written specifically for **Microsoft Azure Developer Associate (AZ-204)** exam preparation.  
> It focuses on **Azure Cosmos DB core concepts, SDK operations, consistency levels, and change feed**, exactly as tested in the exam.

---

## 1. Overview of Azure Cosmos DB

Azure Cosmos DB is a **globally distributed, multi-model NoSQL database service** designed for:
- Low latency (single-digit ms)
- Elastic scalability
- Guaranteed SLAs
- Multiple consistency models

---

## 2. Cosmos DB Resource Hierarchy

```
Azure Account
 └── Cosmos DB Account
      └── Database
           └── Container
                └── Items
```

---

## 3. Perform Operations on Containers and Items Using SDK (.NET)

### Create Client
```csharp
CosmosClient client = new CosmosClient(endpoint, key);
```

### Create Database
```csharp
Database database = await client.CreateDatabaseIfNotExistsAsync("OrdersDb");
```

### Create Container
```csharp
Container container = await database.CreateContainerIfNotExistsAsync(
    "Orders",
    "/customerId",
    400
);
```

### CRUD Operations
```csharp
await container.CreateItemAsync(item, new PartitionKey(item.customerId));
await container.ReadItemAsync<Order>(id, new PartitionKey(customerId));
await container.ReplaceItemAsync(item, item.Id, new PartitionKey(item.customerId));
await container.DeleteItemAsync<Order>(id, new PartitionKey(customerId));
```

---

## 4. Consistency Levels (Detailed)

### Strong
- Linearizability
- Highest latency
- No multi-region writes

### Bounded Staleness
- Lag bounded by time or versions

### Session (Default)
- Client sees its own writes
- Best balance

### Consistent Prefix
- Ordered reads
- May be stale

### Eventual
- Fastest
- No ordering guarantees

---

## 5. Consistency Comparison

| Level | Ordering | Latency | Availability |
|------|----------|---------|--------------|
| Strong | Yes | High | Low |
| Bounded | Yes | Medium | High |
| Session | Client | Low | High |
| Prefix | Yes | Low | High |
| Eventual | No | Lowest | Highest |

---

## 6. Override Consistency Per Request

```csharp
ItemRequestOptions options = new ItemRequestOptions
{
    ConsistencyLevel = ConsistencyLevel.Eventual
};
```

---

## 7. Change Feed

- Tracks inserts & updates
- Ordered
- No deletes

```csharp
FeedIterator<Order> iterator =
    container.GetChangeFeedIterator<Order>(
        ChangeFeedStartFrom.Beginning(),
        ChangeFeedMode.Incremental
    );
```

---

## 8. AZ-204 Exam Tips

- Partition key is mandatory
- Session consistency is default
- Strong consistency limits scalability
- Change Feed is pull-based

---

## End of Notes

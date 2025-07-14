---
author: Nimendra
title: "System Design Notes: Redis Pub/Sub with Go"
date: 2025-07-10
description: "Learn how Redis Pub/Sub works, its limitations, how it compares to Go channels, and how to implement it using Go."
tags: ["redis", "pubsub", "go", "messaging", "distributed-systems"]
categories: ["Backend", "Redis"]
lastmod: 2025-07-10
showtoc: true
TocOpen: false
ShowReadingTime: true
ShowPostNavLinks: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
editPost:
  URL: "https://github.com/nmdra/nmdra.github.io/tree/main/content"
  Text: "Suggest edit"
  appendFilePath: true
--- 

Redis is often described as a **data structure server**. In addition to being a key-value store, Redis offers features such as caching (like Memcached), queues, and **Pub/Sub messaging**.

This article focuses on the **Pub/Sub (publish-subscribe) pattern in Redis**, how it works, its characteristics, and how to use it effectively with Go.

## What is Redis Pub/Sub?

In Redis, a client can **publish** messages to a named channel, and other clients can **subscribe** to receive those messages from that channel.

Redis acts as a message broker — delivering published messages to all connected subscribers of that channel.

```text
PUBLISHER --[msg]--> Redis --> SUBSCRIBER(S)
````

Unlike message queues or Kafka-like systems, **Redis Pub/Sub is synchronous and fire-and-forget**. There is **no persistence** — if no subscriber is connected at the time of publishing, the message is lost.

## Pub/Sub System Components

Redis Pub/Sub involves three key entities:

* **Publisher**: Sends messages to a named channel.
* **Subscriber**: Listens to one or more channels.
* **Channel**: The topic name used to route messages from publishers to subscribers.

> Example: Think of it like a radio show (Publisher) broadcasting over a frequency (Channel). Listeners (Subscribers) must be tuned in at the same time to hear the message.

## Types of Pub/Sub Systems

| Model            | Description                                                                    |
| ---------------- | ------------------------------------------------------------------------------ |
| **Synchronous**  | Sender and receiver must be connected simultaneously. Redis Pub/Sub uses this. |
| **Asynchronous** | Messages are queued/persisted until receivers are available (e.g., Kafka).     |

> Redis Streams, Kafka, RabbitMQ support asynchronous message delivery by persisting messages.

## Redis Pub/Sub with Go

Using the `go-redis` library, Redis Pub/Sub integrates cleanly with Go applications.

### Subscriber Flow

1. Subscribe to a Redis channel:

   ```go
   sub := client.Subscribe(ctx, "mychannel")
   ch := sub.Channel()
   ```

2. Wait for messages on `ch` (a Go channel):

   ```go
   for msg := range ch {
       fmt.Println("Received:", msg.Payload)
   }
   ```

3. Redis keeps the connection open and pushes new messages as they arrive.

### Publisher Flow

1. Publish a message:

   ```go
   client.Publish(ctx, "mychannel", "Hello, Redis!")
   ```

2. All subscribers connected to `"mychannel"` receive the messageinstantly.

## Example Code

### Publisher.go

```go
var ctx = context.Background()

func main() {
	client := redis.NewClient(&redis.Options{
		Addr: "localhost:6379",
	})
	defer client.Close()

	channel := "mychannel"

	for i := 1; i <= 5; i++ {
		message := fmt.Sprintf("Message %d", i)
		err := client.Publish(ctx, channel, message).Err()
		if err != nil {
			log.Fatalf("Publish failed: %v", err)
		}
		log.Printf("Published: %s", message)
		time.Sleep(4 * time.Second)
	}
}
```

### Subscriber.go

```go
var ctx = context.Background()

func main() {
	client := redis.NewClient(&redis.Options{
		Addr: "localhost:6379",
	})
	defer client.Close()

	sub := client.Subscribe(ctx, "mychannel")
	ch := sub.Channel()

	for msg := range ch {
		log.Printf("Received message: %s", msg.Payload)
	}
}
```
{{<figure src="/images/Redis PubSub and Implementation.webp" caption="" alt="pubsub demo" width= "100%" height="auto"  align="center" >}}

## Summary

Redis Pub/Sub is a fast, lightweight, real-time messaging system suited for distributed applications that require **low-latency communication** without persistence.

However, for mission-critical systems needing **durability, retries, and acknowledgments**, consider message queues (like RabbitMQ) or streams (like Kafka or Redis Streams).

## References

* Build Your Own Redis (CodeCrafters): [codecrafters.io](https://app.codecrafters.io/r/gentle-armadillo-148010)
* [Redis Pub/Sub - Official Docs](https://redis.io/glossary/pub-sub/)
* [Redis Pub/Sub In-Depth (Medium)](https://medium.com/@joudwawad/redis-pub-sub-in-depth-d2c6f4334826)
* [Go Redis Client - GitHub](https://github.com/redis/go-redis)
* [Publish/Subscribe Explanation - StackOverflow](https://stackoverflow.com/a/9562169)


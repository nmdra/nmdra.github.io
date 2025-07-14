---
author: Nimendra
title: "System Design Notes: Split Brain in Docker Swarm"
date: 2025-07-14
description: "Understand the causes, consequences, and prevention of split-brain scenarios in Docker Swarm using Raft consensus and quorum-based leader election."
tags: ["docker", "distributed-systems", "raft", "quorum", "K8s"]
categories: ["Backend", "Docker"]
lastmod: 2025-07-14
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

**Split Brain** in distributed systems, such as Docker Swarm, occurs when a network partition causes nodes to lose communication with one another.

This results in two or more subsets of nodes thinking they are the **leader** or **primary controller** of the cluster. This inconsistency can lead to:

- Data corruption  
- Conflicting operations  
- Duplicate tasks being executed

## How it Happens

1. **Network Partition**: A temporary network failure splits the nodes into two or more isolated groups.
2. **Leader Election Conflict**: Each isolated group might independently attempt to elect a leader.
3. **Independent Decisions**: Each group operates as a separate cluster, leading to inconsistent states.

> ### In a Docker Swarm cluster:
>
> - Nodes are classified into **managers** and **workers**.
> - **Managers** coordinate service orchestration and maintain the cluster state.
> - If a partition occurs:
>   - Each group of managers may elect its own leader.
>   - This results in multiple active leaders (**split brain**) and **service conflicts**.

## Consequences of Split Brain

1. **Data Inconsistency**: Multiple leaders might make conflicting updates.
2. **Duplicate Workloads**: Services may be scheduled redundantly.
3. **Unrecoverable State**: Independent decisions by both partitions can be hard to reconcile.
4. **Reduced System Reliability**: The system becomes unpredictable or unusable.

## Prevention Techniques in Docker Swarm

Docker Swarm uses the following techniques to avoid split-brain scenarios:

1. **Raft Consensus Algorithm**  
   Ensures only one leader exists by requiring majority agreement.

2. **Quorum Enforcement**  
   A cluster will only elect a leader and make decisions if the majority (quorum) of manager nodes are reachable.

3. **Network Redundancy**  
   By avoiding partitions via redundant network paths, clusters reduce the risk of isolation.

## Quorum Explained

### 1. Majority Rule

A quorum is achieved when **more than half** of manager nodes agree.

| Managers | Quorum (Majority) | Fault Tolerance |
|----------|-------------------|------------------|
| 1        | 1                 | 0                |
| 2        | 2                 | 0                |
| 3        | 2                 | 1                |
| 4        | 3                 | 1                |
| 5        | 3                 | 2                |
| 6        | 4                 | 2                |
| 7        | 4                 | 3                |

- **Example**: In a 3-manager node setup, 2 must be online to form a quorum.

When a network partition occurs:

- The **partition with quorum** (majority) becomes the **active** cluster.
- The minority partition becomes **inactive** or **read-only**.

### 2. Leader Election and Heartbeats

- Manager nodes use **heartbeat messages** to monitor each other's health.
- When heartbeats fail, managers assume the leader is down and initiate **leader election** via Raft.

> ### What are heartbeats?
>
> A **heartbeat** in Docker Swarm is a periodic signal sent between manager nodes to detect node failure. This ensures only active, reachable nodes participate in orchestration.

#### Rules for Leader Election

1. A manager can **only become a leader if it has quorum**.
2. If quorum is not met, **no leader is elected**, and the cluster **pauses operations**.


## Example Scenarios

### Scenario 1: 3 Manager Nodes

- Partition A: 2 nodes → **quorum met**
- Partition B: 1 node → **no quorum**

➡ Partition A remains active; Partition B becomes read-only.

### Scenario 2: 4 Manager Nodes

- Partition A: 2 nodes  
- Partition B: 2 nodes  

➡ Neither side has quorum (majority = 3), causing the system to **pause orchestration** and potentially enter **split-brain** until resolved.

# Split-Brain Scenarios on K8s

In Kubernetes, the control plane relies on a distributed key-value store called etcd, which stores the entire cluster state—pods, configurations, secrets, and more. To ensure consistency and fault tolerance, etcd uses the Raft consensus algorithm, which helps prevent split-brain scenarios.

👉 [Learn how etcd works](https://learnk8s.io/etcd-kubernetes)





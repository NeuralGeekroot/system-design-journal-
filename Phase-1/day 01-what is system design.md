# Today's Concept
1. What is System Design?
2. HLD vs LLD vs Infrastructure — The 3 Levels of Design
3. Why This Matters?

---

## Resources
1. ByteByteGo — "What is system design?" — https://www.youtube.com/@ByteByteGo
2. System Design Primer (Introduction only) — https://github.com/donnemartin/system-design-primer
3. roadmap.sh — System Design overview — https://roadmap.sh/system-design
4. "The Complete Guide to System Design" — https://dev.to/fahimulhaq/complete-guide-to-system-design-oc7

---

## 1. What is System Design?

System design is the process of turning a vague requirement into a concrete architecture that a team can build, operate, and grow over time.

It means breaking a problem into smaller components, deciding how they work together, and making smart trade-offs under real-world constraints — scale, latency, reliability, and cost.

> **Example:** "Build a URL shortener" is vague. System design answers: Who uses it? How many requests per second? Where is data stored? What happens if the database goes down?

**The core idea:** System design is about making the right trade-offs under constraints. The goal is always the *simplest* architecture that satisfies the requirements.

---

## 2. HLD vs LLD vs Infrastructure

| Level | What it covers | Question it answers |
|---|---|---|
| **HLD** (High-Level Design) | Components, interactions, data flow | *What are the parts and how do they connect?* |
| **LLD** (Low-Level Design) | Class diagrams, schemas, algorithms | *How does each part work internally?* |
| **Infrastructure** | Servers, cloud, networking, monitoring | *Where and how does it run?* |

> **Example:** For a food delivery app — HLD shows the order service, payment service, and notification service; LLD shows the database schema for orders; Infrastructure shows it runs on AWS with auto-scaling.

---

## 3. Why This Matters?

- Real systems fail at scale if the design is poor — slow responses, data loss, downtime.
- Every senior engineering role involves architecture decisions. System design is that skill.
- In interviews, it's how companies evaluate if you can think beyond just writing code.

---

## What System Design Covers

A typical system design discussion revolves around these questions:

| Area | Question |
|---|---|
| **Requirements** | What should the system do? What's out of scope? |
| **Scale** | How many users, requests, reads, writes, and how much data? |
| **Data** | What is stored, where, and how is it accessed? |
| **APIs** | How do clients and services communicate? |
| **Reliability** | What happens when a component fails? |
| **Performance** | What latency and throughput targets matter? |
| **Consistency** | Which operations need strong correctness vs. eventual? |
| **Operations** | How is the system monitored, deployed, and evolved? |
| **Cost** | Is the design reasonable for the expected scale? |

The same product can have very different designs depending on its constraints.

---

## Typical Components

Most systems share these building blocks:

- **Client** — Web app, mobile app, or service that sends requests
- **API layer** — The interface clients use to interact with the system
- **Load balancer** — Distributes incoming traffic across multiple servers
- **Application servers** — Run business logic, coordinate reads and writes
- **Cache** — Stores frequently accessed data to reduce DB load and improve latency
- **Database/Storage** — Persists the system's durable data
- **Message queue** — Decouples producers and consumers; handles async work
- **External services** — Payment providers, email/SMS, identity, analytics
- **Observability stack** — Logs, metrics, traces, alerts, dashboards

> System design is less about listing these components and more about explaining **how requests and data flow through them**.

---

## How to Approach System Design (General)

### Step 1 — Clarify Requirements

Before choosing an architecture, understand the problem. Don't jump straight into a solution.

Ask:
- What are the core features (in priority order)?
- Who are the users?
- What is explicitly out of scope?
- What scale to design for? (users, requests/sec, data size)
- Which operations need low latency or strong correctness?
- What availability or durability is expected?

> **Example:** "Users can view a timeline" is not enough. A timeline for 10,000 users is designed very differently from one serving 500 million users with a 200ms latency target.

---

### Step 2 — Estimate Scale

Rough numbers help avoid building the wrong system. Estimate:
- Read/write requests per second
- Storage growth over time
- Bandwidth requirements
- Peak traffic patterns
- Hot spots or uneven access patterns

> **Aim:** Not exact math — just enough to spot the design pressure. 100 writes/sec vs 1 million writes/sec will have completely different bottlenecks.

---

### Step 3 — Start Simple

Begin with the minimum design that works:

```
Client → API layer → Application server → Database
```

Add complexity only when there's a clear reason:

| Add this | When... |
|---|---|
| Cache | Repeated reads overload the DB, or latency is too high |
| Message queue | Work can be async, or traffic spikes need buffering |
| Read replicas | Read traffic grows, or availability needs redundancy |
| Partitioning/Sharding | One machine can no longer hold or serve all the data |
| CDN | Users are geographically spread out |

Real systems evolve one constraint at a time.

---

### Step 4 — Identify Bottlenecks and Failure Modes

Ask: *What breaks under load? What breaks if a component fails?*

- What if the database goes down?
- What if the cache is empty (cold start)?
- What if a queue consumer falls behind?
- What if a third-party API becomes slow or unavailable?

A good design acknowledges these failures and explains how the system behaves when they happen.

---

### Step 5 — Discuss Trade-offs

Every meaningful design choice has a cost:

| Choice | Benefit | Cost |
|---|---|---|
| Caching | Reduces latency | Cache invalidation becomes complex |
| Replication | Better availability + read throughput | Consistency lag between replicas |
| Sharding | Higher write capacity | Cross-shard queries become harder |
| Async processing | Absorbs traffic spikes | Users don't see results immediately |

Always explain **both sides**: why a choice helps and what new problem it creates.

---

## Interview Framework

In a system design interview, follow this structure and time allocation:

| Phase | Time | Goal |
|---|---|---|
| Clarify requirements | 5–10 min | Understand the problem, establish scope |
| High-level design | 20 min | Propose architecture and get buy-in |
| Deep dive | 15 min | Drill into critical components |
| Wrap up | 5 min | Ask meaningful questions |

---

### Phase 1 — Clarify Requirements

Questions are intentionally vague. Ask about features, users, and non-functional requirements. Focus especially on **scale and performance**.

> Example: In a chat app design, clarify — is it 1:1 or group chat? Does it need offline messages? Real-time? How many users?

---

### Phase 2 — High-Level Design

Follow this top-down order:

1. **API Design** — Define what the system exposes (the contract between components). Answer *what* the system does before worrying about *how*.
2. **Architecture Diagram** — Sketch major components (services, databases, queues, caches) and how they connect. Gives everyone a shared mental model.
3. **Data Model** — Decide how data is structured, stored, and accessed. Right schema choices here prevent painful migrations later.

---

### Phase 3 — Deep Dive

For each critical component, repeat this loop:

1. Articulate the problem
2. Come up with at least two solutions
3. Discuss trade-offs of each
4. Pick a solution and justify it
5. Move to the next component

---

### Phase 4 — Wrap Up

Ask meaningful questions — not softballs:
- What tech stack does the team use?
- What engineering challenges are they currently facing?
- What would you want me to focus on in the first 30/60/90 days?

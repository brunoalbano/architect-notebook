# ROADMAP — Path to Software Architect

The topic backlog. Grouped into **tiers**; do tiers roughly in order (fundamentals first), but
topics *within and across reachable tiers are independent* — pick by interest, since the schedule
is irregular. Goal is **deep mastery**, so *own* ~30 of these deeply (real decision cards + ADRs)
rather than skimming all of them. Check off as you finish each loop stage.

Legend per topic: `[ ] Read · Model · Build · Decide · Write` (tick when done).

---

## Canonical sources (the spine — map topics to these)
- **FoSA** — *Fundamentals of Software Architecture* — Richards & Ford
- **Hard Parts** — *Software Architecture: The Hard Parts* — Ford, Richards, Sadalage, Dehghani
- **DDIA** — *Designing Data-Intensive Applications* — Kleppmann _(foundational; nearly a course on its own)_
- **EIP** — *Enterprise Integration Patterns* — Hohpe & Woolf
- **DDD** — *Domain-Driven Design* (Evans) + *Implementing DDD* (Vernon) _(you know this — use for depth)_
- **Release It!** — Nygard _(stability/resilience patterns, real postmortems)_
- **Building Microservices** — Newman
- **Team Topologies** — Skelton & Pais _(Conway's Law made actionable; team boundaries as architecture)_
- **HPBN** — *High Performance Browser Networking* — Ilya Grigorik _(free online; reads-first, architecture-relevant networking)_
- **C4 Model** — c4model.com (Simon Brown) · **ADRs** — Michael Nygard's original post + adr.github.io
- Official docs: Microsoft **Azure Architecture Center** (incl. Cloud Design Patterns & Well-Architected
  Framework), AWS Well-Architected — primary go-to for managed-service tradeoffs.

> Claude curates exact chapters per topic at session start and summarizes dense docs into `_sources/`.

---

## Tier 0 — Foundations of the craft  *(do first; everything hangs off this)*
- [ ] **00 · Architecture fundamentals & quality attributes** — the tradeoff vocabulary, architecture
  characteristics, fitness functions. *(scaffolded — start here)* · FoSA ch.1–7
- [ ] **01 · Architecture Decision Records (ADRs)** — the daily artifact; practice the format itself.
  Moved ahead of diagramming on purpose: you write one every topic from 00 on. *(scaffolded — folder ready)*
- [ ] **02 · Documenting architecture: the C4 model** — context / container / component / code; the
  modeling *method* (what to draw and at which zoom level), not the drawing tool. · C4 Model
- [ ] **03 · Diagramming & whiteboarding tools** — Excalidraw, draw.io, Mermaid, PlantUML, Structurizr DSL;
  pick a primary and learn it cold for design drills. *(the tooling for topic 02; reference: `_drills/tooling.md`)*
- [ ] **04 · Architecture styles overview** — layered, modular monolith, microkernel/plugin,
  event-driven, microservices, space-based, pipeline. When each fits. · FoSA ch.9–17

## Tier 1 — Distributed systems fundamentals  *(the hard core)*
> Foundational companion: **Tier 9 (networking)** silently underpins this tier — the fallacies below
> *assume* you can reason about latency, partitions and DNS. If that's shaky, pull 90–97 alongside this.
- [ ] **10 · The 8 fallacies of distributed computing** + why distribution is a last resort.
- [ ] **11 · Consistency, availability & CAP/PACELC** — what "eventual consistency" actually costs. · DDIA ch.5,9
- [ ] **12 · Replication & partitioning/sharding** · DDIA ch.5–6
- [ ] **13 · Consensus & coordination** — leader election, quorums, Raft (conceptual). · DDIA ch.8–9
- [ ] **14 · Idempotency, retries, timeouts, backoff** — the resilience primitives.
- [ ] **15 · Resilience patterns** — **circuit breaker** (closed/open/half-open states, thresholds, fallback)
  as the marquee pattern; plus bulkhead, rate limiting, load shedding, graceful degradation. Builds on the
  topic 14 primitives. Spike with **Polly** (.NET). · Release It!

## Tier 2 — Data architecture
- [ ] **20 · OLTP vs OLAP; relational vs document vs key-value vs columnar vs graph** — picking a store. · DDIA ch.2–3
- [ ] **21 · Transactions, isolation levels, locking** ⚠️ — what your DB *actually* guarantees: phantom
  reads, write skew, lost updates, why your ORM's default isolation bites under load. *(Don't auto-skip —
  "I write SQL" ≠ owning concurrency guarantees.)* · DDIA ch.7
- [ ] **22 · Event sourcing** — benefits, traps, when it's overkill.
- [ ] **23 · CQRS** — read/write split, sync lag, when it's premature.
- [ ] **24 · Caching strategies** — cache-aside/read-through/write-through, invalidation, Redis on Azure.
- [ ] **25 · NoSQL data modeling deep-dive** — query-first / denormalized modeling in a wide-column and a
  document store (**Cassandra** + **MongoDB / Azure Cosmos DB**); partition-key & access-pattern design,
  secondary indexes, and the **failure modes** (hot partitions, tombstones, unbounded item growth, fan-out
  writes). The hands-on counterpart to topic 20's "picking a store" — *how to actually model* once you've
  picked NoSQL. *(builds on 11, 12, 20)*
- [ ] **26 · Scaling the data tier — the read/write scaling ladder** — the sequenced playbook for taking a
  database from one box to hundreds of thousands of users: **index → cache → read replicas → vertical scale
  → functional partitioning → shard → relax consistency**, and *why that order*. Capacity-planning math
  (users → QPS → IOPS/connections), plus the **failure modes** of each rung: replication lag, connection-pool
  exhaustion, cross-shard queries & joins, rebalancing/resharding pain, the cache-stampede & thundering-herd
  traps. The synthesis topic that ties 11·12·23·24·25 into one decision sequence. *(builds on 11, 12, 23, 24, 25; pairs with 53)*
- [ ] **27 · Multi-region data** — geo-distributing the data tier for latency and survival: **active-passive
  vs active-active**, single-writer/global-read vs multi-writer, async geo-replication & the **RPO/RTO**
  it buys, write-locality strategies (home-region/geo-partitioning by tenant), and **conflict resolution**
  (LWW, CRDTs, app-level merge) once two regions accept writes. The hard tradeoffs: cross-region latency
  (speed-of-light floor), split-brain & failover, and **data residency/sovereignty** as a placement
  constraint. Concrete: **Cosmos DB multi-region**, Azure SQL geo-replication; one-line AWS equivalents
  (Aurora Global, DynamoDB Global Tables). *(builds on 11, 12, 26; pairs with 54 reliability, 52 governance, 96)*

> *Sagas* moved to Tier 3 (needs messaging first) and *data ownership / database-per-service* moved to
> Tier 4 (needs service boundaries first) — the dual-write family (22·23·33·34) spans Tiers 2–3, study it as a set.

## Tier 3 — Integration, messaging & APIs
- [ ] **30 · Sync vs async communication** — coupling, latency, failure semantics.
- [ ] **31 · Messaging & brokers** — queues vs topics, Azure Service Bus, RabbitMQ, Kafka (log vs queue). · EIP
- [ ] **32 · Event-driven architecture** — events vs commands, choreography, eventual consistency in anger.
- [ ] **33 · Sagas & distributed transactions** — orchestration vs choreography; vs 2PC. *(Lives here, not
  in the data tier: a saga is coordination over async messaging — it needs 31–32 first.)* · Hard Parts
- [ ] **34 · Outbox / inbox & exactly-once-ish delivery** — the dual-write problem.
- [ ] **35 · Integration patterns catalog** — routing, transformation, enrichment. · EIP
- [ ] **36 · API styles: REST vs gRPC vs GraphQL** — pick-by-context. (protocols: HTTP/1.1, HTTP/2, HTTP/3)
- [ ] **37 · API evolution, versioning & contracts** — backward/forward compatibility, versioning
  (URL / header / media-type), deprecation & sunset, **consumer-driven contracts**, schema evolution. The
  part that bites *after* you ship and others depend on you. *(serialization & registry mechanics in topic 82)*
- [ ] **38 · API gateway & BFF pattern.**

## Tier 4 — Service & application architecture
- [ ] **40 · Monolith vs modular monolith vs microservices** — sizing services; distributed-monolith trap. · Building Microservices
- [ ] **41 · Conway's Law & Team Topologies** — organizations ship their communication structure; **team
  boundaries *are* architecture**. Stream-aligned / platform / enabling / complicated-subsystem teams,
  cognitive load as a sizing limit, the inverse-Conway maneuver. The socio-technical force behind every
  decomposition — decide team shape and service shape together. · Team Topologies
- [ ] **42 · Strategic DDD** ⚠️ — bounded contexts, context mapping, subdomain types (core/supporting/
  generic), ubiquitous language as a *boundary* tool. The part of DDD that actually drives decomposition.
  *(You own tactical DDD; strategic is the architect's half — don't auto-skip.)* · DDD (Evans Pt.4), Vernon
- [ ] **43 · Service granularity & decomposition** — sizing/splitting services, the distributed-monolith
  trap. Consumes **42's** domain boundaries and respects **41's** team boundaries. · Hard Parts
- [ ] **44 · Data ownership & the database-per-service boundary** — sharing data without sharing a DB.
  Pairs with 43 (a "database-per-service" only means something once you've decided to have services). · Hard Parts
- [ ] **45 · Clean/Hexagonal/Onion architecture compared** ⚠️ — you've *applied* Clean; go deep on the
  *why*, the limits, and its **failure modes**: over-abstraction, the "where does this logic go?" tax,
  when it's the wrong choice. *(Applying ≠ knowing when not to.)*
- [ ] **46 · Backend-for-frontend (BFF)** — a per-client API-aggregation/tailoring layer (web vs mobile vs
  RN); reduces chatty round-trips and client-side orchestration. When it earns its keep vs when it's an extra
  hop to maintain. *(relates to 36 API styles, 30)*
- [ ] **46a · Micro-frontends** — frontend decomposition so *many teams* ship independently (Conway's Law for
  the UI). The core fork is **composition**: build-time vs server-side vs client-side **runtime (Module
  Federation)** vs iframe/Web Components. Hard problems: shared dependency/version skew (two Reacts on one
  page), bundle bloat, cross-app state & routing, design-system/CSS isolation, independent-deploy vs runtime
  coupling. **When NOT to use** is the whole point — for a *single team* it's all cost, no benefit; the payoff
  is organizational, not technical. Note the **React Native** caveat: web MFE mechanics (DOM, Module
  Federation) mostly don't transfer. *(builds on 41 Conway; relates to 46, 56)*
- [ ] **46b · Presentation-layer architecture (GUI patterns)** — how to structure logic *inside* the UI:
  Forms-and-Controls → MVC → MVP → **Presentation Model / MVVM** → **Supervising Controller** → **Passive
  View**. Don't memorize the zoo — own Fowler's two axes: **observer vs flow synchronization** (implicit/auto
  updates vs explicit/manual) and **how humble the view is** (testability vs indirection cost). Map each to the
  modern stack: React local state + lift-up ≈ flow sync; Redux/Flux ≈ global observer sync; a hook/view-model
  holding view state ≈ Presentation Model; "logic out of the component so it tests without a renderer" ≈
  Passive/Humble View. **When NOT to use:** Passive View's indirection is overkill for a trivial screen; not
  every component needs a view-model. Source: Fowler, *GUI Architectures*. *(relates to 45 Clean Arch, 46a, 62 testability)*
- [ ] **47 · Strangler fig & legacy migration patterns.**
- [ ] **48 · Zero-downtime data & schema migration** — expand/contract (parallel-change), dual-write +
  backfill + cutover, online index builds, backward-compatible schema changes. The hard, unglamorous part
  of evolving a *live* system without a maintenance window. *(relates to topics 21, 37, 47, 56)*
- [ ] **49 · Feature flags / toggles** — decoupling **deploy from release**: the evolvability lever behind
  trunk-based development and progressive delivery. The key insight is the **taxonomy** (Pete Hodgson) —
  release vs experiment (A/B) vs ops/kill-switch vs permission toggles — because each has a *different
  lifespan and owner*, and conflating them is the classic mistake. **Failure modes:** flag debt (stale flags
  never removed), the combinatorial test explosion (N flags → 2^N states), flags hardening into permanent
  in-code branching, an experiment flag that silently becomes load-bearing, and the flag service as a new
  outage/latency dependency. Build-vs-buy: DB-backed homegrown vs **LaunchDarkly / Azure App Configuration**;
  consistent evaluation across services + frontend. *(relates to 56 canary, 48 cutover, 50 observability)*

## Tier 5 — Operability, security & cross-cutting concerns
- [ ] **50 · Observability** — logs, metrics, traces; OpenTelemetry; Azure Monitor / App Insights.
- [ ] **51 · Security architecture** — authn/z, OAuth2/OIDC, JWT, secrets, zero-trust, threat modeling (STRIDE).
- [ ] **52 · Data governance, privacy & compliance** — PII classification, data residency/sovereignty,
  retention & deletion (right-to-be-forgotten), **GDPR/LGPD as architectural constraints**, encryption &
  key ownership, auditability. The regulatory forces that quietly reshape your data design. *(relates to 51, 106)*
- [ ] **53 · Scalability & performance** — vertical/horizontal, statelessness, load patterns, back-pressure.
- [ ] **54 · Reliability** — SLO/SLA/SLI, error budgets, DR, RPO/RTO, multi-region.
- [ ] **55 · Cost as an architecture characteristic** — FinOps, the cost/scalability/availability triangle.
- [ ] **56 · Deployment architecture** — blue/green, canary, feature flags; containers, K8s vs Azure Container Apps.
- [ ] **57 · Service mesh & sidecar infrastructure** — pushing mTLS, retries, traffic-shaping & telemetry
  *out* of app code into the platform; when a mesh (Istio/Linkerd) earns its operational weight vs a
  resilience library (Polly). A decision topic — mechanism lives in topics 82 & 92. *(relates to 15, 50)*
- [ ] **58 · Multi-tenancy — full-stack tenant isolation** — the defining SaaS architecture decision, made
  *consistently across every layer*. The spine is the **silo ↔ pool ↔ bridge** spectrum (dedicated vs shared
  vs hybrid) and the central tradeoff: **isolation/blast-radius/compliance vs cost/operability/density**. Walk
  it layer by layer, deciding the isolation model at each:
  - **Tenant identity & routing** — how a request is mapped to a tenant: subdomain vs path vs header vs token
    claim; tenant context propagation through the call chain.
  - **Frontend** — per-tenant theming/branding/config, custom domains, bundle vs runtime config, RN app per
    tenant vs single app. *(ties to 46)*
  - **DNS & edge** — subdomain-per-tenant vs custom domains (`app.acme.com`), wildcard certs vs per-tenant TLS
    (ACME automation), CDN/edge routing & cache-key-by-tenant. *(ties to 90–96, 95)*
  - **Services / compute** — shared pool vs per-tenant instances; tenant context in auth (scoping every query),
    quotas & **noisy-neighbor** throttling, per-tenant rate limits. *(ties to 15, 51)*
  - **Data** — the relational/SQL options first since that's the common case: shared schema + `tenant_id`
    (+ **row-level security** to enforce isolation in-engine) vs schema-per-tenant vs database-per-tenant vs
    shard-by-tenant. Concrete on **SQL Server / Azure SQL**: RLS security policies, **elastic pools** to share
    capacity across many per-tenant DBs, and the **EF Core** global-query-filter pitfall (a forgotten filter =
    cross-tenant leak). Then the NoSQL/Cosmos analogue (partition-key = tenant). Plus migrations &
    backup/restore *per tenant*, and the noisy-neighbor & blast-radius math. *(ties to 26, 27, 21)*
  - **Infra & ops** — landing-zone/subscription-per-tenant vs shared, IaC parameterized by tenant, per-tenant
    **cost attribution** & metering, onboarding/offboarding (including hard-delete for compliance). *(ties to 52, 55, 65)*
  Heavy on **failure modes**: cross-tenant data leakage (the cardinal sin), a "pool" tenant that outgrows the
  pool, per-tenant cost blindness, and the migration trap of moving a big tenant silo→pool or back. *(builds on 26, 27; relates to 46, 51, 52, 54, 55, 65)*

## Tier 6 — Architect tooling, frameworks & the role itself
- [ ] **60 · Architecture description & modeling tools** — C4/Structurizr, the arc42 template, Mermaid,
  PlantUML, diagrams-as-code, Azure DevOps/Visio. Hands-on on Windows + VS Code.
- [ ] **61 · Architecture evaluation methods** — ATAM, lightweight risk-storming, fitness functions / evolutionary
  architecture. · *Building Evolutionary Architectures* (Ford et al.)
- [ ] **62 · Testing strategy & testability as architecture** — the test pyramid at system scale,
  **contract testing (Pact)** for service boundaries, testability as a first-class quality attribute, and
  how **fitness functions (61)** turn architectural rules into automated tests. *(relates to 37, 61)*
- [ ] **63 · Frameworks & reference architectures** — Azure / AWS Well-Architected, TOGAF (awareness-level),
  the C4 + arc42 combo as a working documentation framework.
- [ ] **64 · The soft skills of the role** — tradeoff communication, stakeholder management, writing for
  decisions, leading without authority, the architect-as-mentor. · FoSA ch.21–24
- [ ] **65 · Cloud-native & platform topics** — IaC at architecture scale (you have Terraform/Spacelift),
  well-architected reviews, landing zones. *(multi-tenancy now lives in topic 58)*
- [ ] **66 · Multi-cloud & hybrid architecture strategy** — the "why" and "how" *before* the network plumbing
  in topic 96: single-cloud-by-default vs multi-cloud-by-design (avoid lock-in, best-of-breed, regulatory/
  M&A-driven) vs hybrid (cloud + on-prem/private DC for latency, legacy, sovereignty, or cost reasons).
  Decision drivers: portability cost (abstraction layers eat the differentiated features you pay for),
  data-gravity & egress cost as the real multi-cloud tax, identity federation across clouds, and
  observability/ops fragmentation (N monitoring stacks). Patterns: active-active vs active-passive
  across providers, workload placement (what *must* stay on-prem — data residency, latency to legacy
  systems — vs what's cloud-native), Kubernetes as the common substrate (portable compute, not portable
  data), and abstraction tradeoffs (Terraform/Crossplane multi-provider IaC vs cloud-native services
  directly). **Failure modes:** the "lowest common denominator" trap (multi-cloud without a real driver
  just multiplies ops cost), split identity/observability, egress bills that dwarf compute savings,
  and hybrid connectivity as a new single point of failure (ExpressRoute/Direct Connect/VPN). Concrete:
  Azure Arc / AWS Outposts for hybrid; one-line note on GCP Anthos. *(relates to 55 cost, 65 IaC, 96
  networking; builds the strategy layer that 96's VNet/VPC mechanics implement)*

## Tier 7 — Frontier / context-dependent  *(pick if relevant to real work)*
- [ ] **70 · Data pipelines, lakehouse & streaming architecture** — the full data-platform stack behind
  "Data & AI teams" in a modern org, not just Kafka mechanics. Covers:
  - **Batch vs streaming vs micro-batch** ingestion, and the **Lambda vs Kappa architecture** debate
    (why most teams converge on Kappa/streaming-first today).
  - **Medallion architecture** (bronze/silver/gold) and the **data lake → lakehouse** evolution — why
    lakehouse (Delta Lake / Apache Iceberg / Hudi table formats: ACID + schema evolution on object
    storage) replaced the lake-vs-warehouse either/or. Lakehouse vs traditional data warehouse
    (Synapse/Redshift/Snowflake) vs pure lake — when each still wins.
  - **CDC (Change Data Capture)** as the bridge from OLTP into the pipeline (ties to outbox pattern, 34).
  - **Orchestration**: Azure Data Factory / Databricks Workflows / Airflow — DAG-based batch orchestration
    vs event-driven triggering.
  - **Data contracts & schema governance** — producer/consumer contracts on pipeline data, schema
    registry (ties to 37, 82), and *why* this is where most "AI-ready data" initiatives actually fail
    (garbage upstream, no contract enforcement).
  - **Data mesh (awareness)** — domain-oriented data ownership, data-as-a-product, federated governance;
    when it's the right org-scale answer vs when centralized lakehouse is simpler.
  - **Failure modes:** schema drift breaking downstream consumers silently, small-file problems on object
    storage, unbounded reprocessing cost, stale/duplicate data from CDC gaps, and pipelines becoming
    unowned "spaghetti ETL" without contracts. Concrete: **Azure Databricks + ADLS Gen2 + Synapse**; one-line
    AWS equivalent (Glue/EMR + S3 + Redshift). *(relates to 20, 22, 34, 37, 52 governance, 108 MLOps boundary)*
- [ ] **71 · AI/LLM in architecture** — *expanded into its own **Tier 10** below.*
- [ ] **72 · Edge, IoT, real-time** — if/when a project demands it.

## Tier 8 — Protocols, standards & tool-selection deep-dives  *("which one do I pick", the non-obvious)*
The decisions that don't live in any book chapter — concrete "X vs Y vs Z" calls you'll defend in design
reviews. Each is a focused decision card with a comparison table at its heart. Pull by interest/need.
- [ ] **80 · Identity & access protocols: OAuth2 vs OIDC vs SAML vs SCIM** — authZ vs authN vs federation
  vs provisioning (they solve *different* problems). OAuth2 grant types (auth-code+PKCE, client-creds,
  device), OIDC on top of OAuth2, SAML for enterprise SSO, **SCIM** for user provisioning/deprovisioning.
  When each, and how they combine in a real enterprise SSO + lifecycle setup. (relates to topic 51)
- [ ] **81 · "What's my queue?" — SQS vs SNS vs Kafka vs RabbitMQ vs Azure Service Bus vs Postgres
  `SKIP LOCKED`** — log vs queue vs pub/sub vs "just use the DB". Ordering, delivery guarantees, replay,
  throughput, retention, ops cost. The key insight: most teams reach for Kafka when a transactional
  outbox + `SELECT … FOR UPDATE SKIP LOCKED` would do. When the DB-as-queue is right and when it melts. (relates to topics 31, 34)
- [ ] **82 · Non-obvious protocols & tools catalog** — a *menu* of small, sharp deep-dives; do one when it
  becomes relevant. Candidates (add/remove freely):
  - Real-time transport: **WebSockets vs SSE vs long-polling vs webhooks vs gRPC streaming**
  - Serialization/schema: **Protobuf vs Avro vs JSON vs MessagePack**; schema registry; **CloudEvents**
  - API description: **OpenAPI vs AsyncAPI**; contract testing
  - Tokens: **JWT vs opaque vs PASETO**; sessions vs tokens; webhook **HMAC signing**; **Idempotency-Key** header
  - Distributed locks: **Redis (Redlock) vs etcd/Zookeeper vs Postgres advisory locks**
  - Rate-limiting algorithms: **token bucket vs leaky bucket vs sliding window**
  - Service mesh (Envoy/Istio/Linkerd) vs library resilience; **mTLS** & TLS termination *(decision topic: 57)*
  - Change data capture: **Debezium/CDC vs outbox**; **consistent hashing**; probabilistic structures (Bloom/HLL)
  - Enterprise identity plumbing: **LDAP / Active Directory / Kerberos**; **Reactive Streams** back-pressure

## Tier 9 — Networking for architects  *(high-priority foundation — do roughly IN ORDER; it builds bottom-up)*
> **On placement:** networking is *foundational*, not advanced — its high tier number reflects when it was
> added to this roadmap, **not** when to study it. Treat it as an **early pull (right after Tier 1)**: the
> fallacies (10), resilience (15), scalability (53) and load balancing all silently assume it. The "do
> tiers in order" rule bends here — it's a high-priority foundation.

Unlike other tiers, do these sequentially — each layer assumes the one below. Goal isn't CCNA-style
depth; it's enough to reason about latency, failure, and topology in a design review. Spike with real
tools on Windows: `curl -v`, `Test-NetConnection`, `Resolve-DnsName`, browser DevTools → Network, Wireshark.
- [ ] **90 · How a request actually travels** — OSI/TCP-IP layers, what happens end-to-end when you hit a
  URL (DNS → TCP → TLS → HTTP → response). The mental model everything else hangs off. · HPBN ch.1
- [ ] **91 · TCP vs UDP & the connection lifecycle** — handshake, slow start, congestion control,
  head-of-line blocking, why connection setup is a latency tax. When UDP (and QUIC) instead. · HPBN ch.2
- [ ] **92 · TLS/SSL in depth** — handshake cost, certificates & chains, SNI, **mTLS**, TLS termination
  (where to terminate and why), session resumption. · HPBN ch.4
- [ ] **93 · DNS deep-dive** — resolution path, record types, TTL & caching, GeoDNS, **Anycast**, failover;
  DNS as a load-balancing and outage vector (it's in half the postmortems). Spike: `Resolve-DnsName`.
- [ ] **94 · HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC)** — multiplexing, head-of-line blocking at TCP vs app
  layer, server push's death, why HTTP/3 moved to UDP. Maps to topic 36. · HPBN ch.11–13
- [ ] **95 · Load balancing & proxies** — **L4 vs L7**, algorithms (round-robin/least-conn/hashing),
  sticky sessions vs stateless, reverse proxies, **CDN & edge**, health checks. (relates to topics 53, 56)
- [ ] **96 · IP, subnets, NAT & cloud network topology** — CIDR/subnetting, private vs public, NAT,
  **Azure VNet** (subnets, NSGs, peering, **Private Link/Private Endpoint**, App Gateway, **Front Door**,
  ExpressRoute) + one-line AWS VPC equivalents. The networking you design in IaC. (relates to topic 65)
- [ ] **96a · IP/CIDR & subnet math** ⚠️ — *drill to reflex.* Calculate address ranges,
  block sizes, masks, network/broadcast via the "magic number" method; split blocks; the Azure/AWS
  "5 reserved per subnet" gotcha. Pure muscle memory — learn the method, then do timed practice.
  📄 Cheat sheet ready: [`_sources/96-ip-subnetting/cheatsheet.md`](_sources/96-ip-subnetting/cheatsheet.md)
- [ ] **97 · Network performance & failure** — latency vs bandwidth vs throughput vs RTT, the speed-of-
  light floor, bufferbloat, timeouts/retries at the network layer, partial partitions. Ties back to the
  distributed-systems fallacies (topic 10) and resilience (topic 15).

## Tier 10 — AI systems architecture  *(current & high-leverage; Azure-first given your stack)*
Architecting systems that *use* LLMs/ML, not training models. The new tradeoffs are **non-determinism,
token cost/latency, context limits, and a fuzzy correctness boundary** — treat the model as an unreliable,
expensive, stateless dependency. Verify vendor/model specifics against live docs (this space moves monthly).
- [ ] **100 · LLM application patterns & when each fits** — raw prompting vs **RAG** vs tool-using **agents**
  vs fine-tuning. The decision tree: most "we need fine-tuning" is actually "we need better retrieval/prompting".
- [ ] **101 · RAG architecture** — ingestion → chunking → **embeddings** → vector store → retrieval →
  rerank → prompt assembly. Failure modes: bad chunking, stale index, retrieval misses, context stuffing.
- [ ] **102 · Vector stores & search** — **pgvector vs Azure AI Search vs Pinecone/Qdrant/Milvus**;
  hybrid (keyword + vector) search, ANN index tradeoffs (HNSW/IVF), when a plain DB or BM25 beats vectors.
- [ ] **103 · Agentic architectures** — tool/function calling, orchestration, planning loops, multi-agent
  vs single-agent, **MCP (Model Context Protocol)**, when agents are over-engineering vs a fixed pipeline.
- [ ] **104 · LLM cost, latency & token economics** ★ — tokens as a first-class architecture characteristic;
  prompt **caching**, streaming, batching, model routing (small-cheap vs large-capable), context-window budgeting.
- [ ] **105 · Evaluation, observability & quality** — offline **evals**, LLM-as-judge, tracing/token metrics,
  regression suites for prompts; how you even define "correct" for a probabilistic system.
- [ ] **106 · AI safety & security architecture** — **prompt injection** (esp. with tools/RAG), data
  exfiltration, PII handling, output guardrails, sandboxing tool calls, human-in-the-loop. (relates to topics 51, 52)
- [ ] **107 · Hosting & inference topology** — Azure OpenAI / managed APIs vs self-hosted OSS models;
  provisioned vs pay-per-token, data residency, rate limits/quotas, fallbacks; **Semantic Kernel** (.NET).
- [ ] **108 · Data & MLOps boundary (awareness)** — feature stores, model/prompt versioning, CD for models,
  drift; where the AI system meets your normal data architecture (topics 20–24, 70).

---

## System design drills (mock interviews)
Run one roughly every **4–6 completed topics** (or any checkpoint): say *"run a system design drill"*
and Claude acts as interviewer, scores you, and logs it. See `_drills/`. Drills integrate topics and
expose whether the judgment holds under pressure — the closest proxy to the real architect job.

## Real-world case studies
From time to time, run **`/case-study`** for a real architecture decision from the wild — biased toward
"company dropped a popular pattern/tool and why". Best source of "when NOT to use" judgment. See `_case-studies/`.

---

## Progress at a glance
- **Active topic:** 00 · Architecture fundamentals
- **Completed:** —
- **Capstone status:** not started (kick off after ~tier 0–1)
- **Revisit queue:** `_journal/revisit.md` — re-attack old decisions once their trigger topic lands (this
  is the "no-rush advantage"; don't let it rot).

> Next session: open `00-architecture-fundamentals/NEXT.md` and continue the loop.

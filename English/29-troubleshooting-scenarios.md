# 29 — Troubleshooting Scenarios

## SIEM Engineer Interview Questions

### Topic 01: Disk Full Disasters & Read-Only Cluster Lock

1. At midnight, an indexer node's storage volume reaches 100% capacity, triggering the automated cluster-wide `read_only_allow_delete` lock and dropping all incoming security events; what is your step-by-step emergency troubleshooting procedure to restore ingestion immediately without service downtime?
2. After freeing up disk space, the cluster fails to automatically release the read-only block; what exact REST APIs and configuration commands remove this persistent hard lock, and how do you resolve the underlying root cause?
3. What is the operational distinction between the Low/High Disk Watermarks and the Flood-Stage Watermark, and how do you configure preventive alerting thresholds to avert shard allocation freezes?
4. When 100% disk exhaustion renders standard Linux shell utilities dysfunctional (e.g., bash tab-completion or vim failing due to zero byte availability), which specific temporary directories, core dumps, or JVM heap dumps can be safely pruned as root to reclaim immediate space?
5. When storage bloat is caused by broken retention automation or duplicate backups, what single API command identifies and prioritizes the largest historical indices for offloading or deletion?

### Topic 02: Network Packet Drops & UDP Syslog Loss

6. Out of millions of logs streamed from core network firewalls to the central SIEM syslog listener, approximately 20% of packets are silently dropped; how do you isolate whether drops occur at the network switch layer, the OS socket buffer layer, or within the SIEM application daemon?
7. When Linux diagnostic commands (`netstat -su` or `ss -u -a`) report positive increments in `packet receive errors` and `receive buffer errors`, how do you tune kernel receive socket buffer parameters (`net.core.rmem_max`, `net.core.rmem_default`)?
8. Due to the inherent connectionless limitations of UDP leading to production telemetry loss, what troubleshooting and architecture migration guidelines convert log shipping to TCP/TLS or reliable rsyslog/syslog-ng forwarding?
9. How do you detect and remediate MTU mismatches, packet fragmentation, and dropped jumbo frames occurring across virtual network interfaces (vNIC) or Software-Defined Network (SDN) layers feeding the SIEM collectors?
10. Under high-velocity traffic, how do you mitigate Linux kernel SoftIRQ packet-polling bottlenecks by expanding Network Interface Card (NIC) Ring Buffer sizes using `ethtool`?

### Topic 03: Memory Exhaustion, OOM-Kills & JVM GC Hell

11. An indexer process terminates abruptly without generating application error logs, and the OS syslog reveals `Out of memory: Kill process (java) score ...`; how do you diagnose the root cause behind the Linux kernel Out-Of-Memory (OOM) killer terminating the daemon?
12. Stop-the-world JVM Garbage Collection (GC) pauses exceed 30 seconds, causing the node to flap continuously with `Node disconnected / Master heartbeat timeout` errors; how do you troubleshoot and resolve these severe GC stalls?
13. Why must JVM heap allocation never exceed 50% of physical system RAM (capped at ~31 GB to preserve compressed OOPs), and why is the remaining memory vital for the Linux filesystem PageCache / Lucene segment cache?
14. How do you capture a live Java heap dump from a production node without crashing the service, and how do you analyze it using `jstat` or VisualVM to isolate memory bloat, runaway aggregations, or memory leaks?
15. When the host OS begins memory paging to disk, severely degrading indexing performance, what are the roles of configuring `vm.swappiness = 1` and enforcing `bootstrap.memory_lock: true` in protecting SIEM processes?

### Topic 04: Mapping Explosion & Unparseable Log Chaos

16. The SIEM cluster rejects incoming event batches with the critical error `Limit of total fields [1000] has been exceeded`, causing ingestion failures across thousands of endpoints; what is the emergency troubleshooting workflow to remediate this Mapping Explosion?
17. When two disparate data sources emit identical field names with conflicting data types (e.g., Source A sends `status` as an integer `200` while Source B sends it as a string `"OK"`), how do you resolve the resulting Mapping Conflict in a live ingestion pipeline?
18. Unparsed logs from a newly integrated custom application flood the pipeline, dumping unextracted raw text into `_unparsed` or generic message fields and degrading search performance; what methodology isolates and remediates the parsing pipeline failure?
19. In an ingest node or Logstash pipeline, a flawed Grok pattern suffering from catastrophic regular expression backtracking spikes CPU to 100% and stalls the worker threads; how do you pinpoint and optimize the defective regex?
20. What are the operational trade-offs of disabling dynamic field mapping in favor of strict schema enforcement or runtime fields (`runtime_mappings`) to eliminate schema validation crashes?

### Topic 05: Pipeline Bottlenecks, Backpressure & Worker Stalls

21. Consumer lag across Apache Kafka or buffer queues steadily escalates while indexer node CPU and disk utilization remain low; how do you isolate and debug pipeline bottlenecks and stalled consumer worker threads?
22. When Logstash or Fluentbit worker threads experience severe backpressure and halt ingestion, how do you determine whether the root cause originates from indexer threadpool rejections or network transit latency?
23. If indexer nodes exhibit an escalating rejection count in their `write` threadpools (`es_rejected_execution_exception`), how do you optimize bulk request sizing, batch concurrency, and queue depths?
24. A specialized parsing pipeline suffers a sudden throughput collapse, dropping to a sluggish 100 EPS; how do you profile individual pipeline filters and plugins to identify the specific stage introducing latency?
25. How do you design and configure a Dead Letter Queue (DLQ) to capture malformed or rejected events without blocking the primary ingestion pipeline, enabling offline analysis and re-ingestion?

### Topic 06: Agent Disconnect Storms & Flapping Connections

26. Following an enterprise network routing update, 5,000 endpoint agents concurrently reconnect to the central SIEM manager, triggering a "Thundering Herd" crisis with TLS handshake throttling and connection crashes; how do you mitigate this storm?
27. Endpoint agents across remote branch offices exhibit intermittent flapping, disconnecting and reconnecting every 10 minutes; what systematic diagnostic steps isolate the root cause across firewalls, proxy timeouts, and keep-alive intervals?
28. When an endpoint agent's local SQLite database or file-based spool buffer becomes corrupted, preventing telemetry transmission, how do you remotely purge or repair the cache without reinstalling the agent?
29. A faulty security definition update or OS patch causes the endpoint agent daemon to peg host CPU at 100%, degrading user productivity; what centralized throttling or automated quarantine mechanisms can be deployed?
30. When mutual TLS (mTLS) handshake failures prevent agents from authenticating with the collector, how do you leverage agent diagnostic logs and `openssl s_client` commands to pinpoint the exact certificate verification failure?

### Topic 07: Split-Brain, Cluster Partition & Unassigned Shards

31. A transient datacenter network partition isolates cluster nodes, causing both partitions to elect separate masters and accept divergent writes (Split-Brain disaster); how do you recover cluster quorum and reconcile data inconsistencies?
32. The cluster status transitions to Red with multiple primary shards listed as `UNASSIGNED`; how do you query the `_cluster/allocation/explain` API to uncover the exact underlying cause preventing shard allocation?
33. If physical disk failure permanently destroys the sole copy of a primary shard, what emergency administrative commands (`_cluster/reroute` with `allocate_empty_primary`) bring the cluster back to an operational Yellow/Green status?
34. When cluster master election cycles repeatedly stall or fail, how do you audit and correct quorum configuration parameters (`cluster.initial_master_nodes` / `discovery.zen.minimum_master_nodes`)?
35. System clock drift (NTP synchronization failure) across cluster nodes causes cluster membership drops and replication timeouts; how do you isolate, troubleshoot, and remediate NTP time drift in distributed nodes?

### Topic 08: Slow Search Timeouts & Frozen Dashboards

36. SOC dashboard queries fail with 30-second timeouts while background log ingestion continues normally; how do you inspect running tasks (`_cat/tasks`) to identify and terminate the specific query exhausting search threads?
37. Unindexed leading wildcard searches (e.g., `*password*` or `*admin*`) submitted by analysts trigger massive memory spikes; how do you enforce query validators, search guardrails, and cluster-level query quotas to block harmful syntax?
38. When Fielddata cache memory bloat consumes excessive heap on data nodes, what troubleshooting steps clear the fielddata cache, and how do you enforce keyword-field mapping instead of text-field aggregations?
39. How do you utilize the Search Profiler API to dissect a complex multi-aggregation SIEM query down to the microsecond level, identifying the exact slow query phase?
40. When querying long-term historical logs residing on cold or frozen tiers causes dashboard rendering to freeze, how do you architect and configure Asynchronous Search handling to prevent client timeouts?

### Topic 09: TLS/SSL Certificate Expiration & Handshake Failures

41. An expired inter-node transport TLS certificate halts communication across the entire distributed cluster, taking the SIEM offline; what is the emergency certificate renewal and deployment procedure to restore the cluster with minimal downtime?
42. When endpoint agents fail to establish TLS sessions with the collector, throwing `SSL: CERTIFICATE_VERIFY_FAILED`, what verification steps validate the Certificate Authority (CA) bundle, root trust anchors, and CRL/OCSP endpoints?
43. Following SSL termination at a reverse proxy or load balancer, what methodology troubleshoots cipher suite mismatches, missing SNI headers, or TLS 1.3 protocol incompatibilities on backend forwarders?
44. After issuing updated certificates and private keys, what exact OpenSSL commands verify that the private key modulus matches the public certificate modulus (`openssl rsa -modulus` vs `openssl x509 -modulus`)?
45. Rather than manually copying certificates across dozens of distributed cluster nodes, what automated certificate rotation architecture (e.g., HashiCorp Vault, cert-manager) prevents future expiration disasters?

### Topic 10: Corrupted Indices & File System Crash Recovery

46. Following an ungraceful host reboot or kernel crash, several indices fail to initialize with `CorruptIndexException` errors; what are the operational rules, execution commands, and data loss risks of running the Lucene `CheckIndex` repair tool?
47. If repairing a corrupted index requires truncating damaged Lucene segments, how do you quantify data loss and replay missing logs from upstream Kafka topics or snapshot repositories?
48. When filesystem-level inode or block corruption occurs on underlying ext4 or XFS filesystems, what is the strict recovery protocol for executing `fsck` or `xfs_repair` without causing further corruption to SIEM data directories?
49. If an unrecoverable Translog (transaction log) corruption prevents an index from recovering or opening, what emergency translog truncation procedure restores the index to service?
50. Following a major storage or filesystem recovery event, what metrics, incident timeline, data loss audit, and preventative action items must be included in the executive post-incident engineering report?

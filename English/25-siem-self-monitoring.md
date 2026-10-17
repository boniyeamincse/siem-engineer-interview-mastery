# 25 — SIEM Self-Monitoring

## SIEM Engineer Interview Questions

### Topic 01: Self-Monitoring Principles & Telemetry Architecture

1. Why is self-monitoring mandatory in a production enterprise SIEM, and how can internal platform defects or ingestion bottlenecks effectively blind the entire enterprise security operations center (SOC)?
2. What four architectural tiers of telemetry metrics (OS/Hardware Metrics, JVM/Runtime Metrics, Daemon/Pipeline Metrics, and Database/Cluster Metrics) must be systematically tracked in a comprehensive SIEM self-monitoring framework?
3. How does internal SIEM telemetry collection operate (e.g., OpenSearch/Elasticsearch `_nodes/stats` APIs or Wazuh `wazuh-analysisd` internal performance counters)?
4. To mitigate the "Observer Effect" (preventing monitoring processes from introducing significant CPU overhead or memory contention), how do you tune telemetry sampling frequencies and metric granularity?
5. Telemetry Decoupling: Should internal SIEM health metrics and performance telemetry be ingested into the primary production cluster or routed to an isolated, lightweight dedicated monitoring cluster, and what are the architectural trade-offs?

### Topic 02: Agent Fleet Health & Heartbeat Monitoring

6. How do you design and configure real-time heartbeat and keep-alive telemetry monitoring across large-scale endpoint agent fleets consisting of 500 to 5,000+ endpoints?
7. "Agent Flapping" (endpoints repeatedly disconnecting and reconnecting every minute): How do you automatically detect this instability and diagnose the root cause (e.g., erratic Wi-Fi links, MTU misconfigurations, or host-based security software throttling)?
8. "Stale Agent Discovery": What automation workflows do you implement to identify, tag, and decommission stale or dormant endpoint agents that have failed to communicate for 7 or 30 consecutive days?
9. Endpoint Agent Resource Governance: How do you centrally monitor and alert on abnormal agent behavior, such as sudden CPU spikes or runaway memory consumption on monitored endpoints?
10. Agent Version Drift: How do you construct operational dashboards to track outdated agent versions across diverse operating systems and establish risk-prioritized fleet upgrade schedules?

### Topic 03: Pipeline Throughput, Queue Backlog & Ingestion Lag

11. What metrics and instrumentation do you deploy to measure hop-by-hop real-time throughput EPS across the end-to-end ingestion pipeline (Log Source $\to$ Collector $\to$ Parser $\to$ Indexer)?
12. Queue Backlog Growth & Ingestion Lag: When incoming log velocity outpaces parsing throughput, at what buffer capacity threshold (in megabytes or percentage) should Tier-1 warning alerts and Tier-2 escalation pages trigger?
13. In distributed streaming architectures leveraging Apache Kafka or RabbitMQ, how do you track "Consumer Lag" (the delta between producer offsets and consumer group commits) using Prometheus, Grafana, or specialized monitoring daemons?
14. Ingestion Latency Alerting: What alerting rules do you configure to notify operations when the duration between event generation (`@timestamp`) and search availability in the indexer exceeds a strict 60-second SLA?
15. Ingestion Packet Drop Detection: How do you monitor kernel socket receive buffer overflows (`netstat -s`, `ss -u`) in real time to detect dropped Syslog UDP/TCP packets at the collector layer?

### Topic 04: Indexer Cluster Health & Shard State Telemetry

16. How do you poll and parse the `_cluster/health` API (monitoring Green, Yellow, and Red operational states) into external monitoring platforms to maintain continuous visibility?
17. What precise conditions cause an indexer cluster state to transition to Yellow (e.g., unassigned replica shards) versus Red (e.g., missing or corrupted primary shards), and what are the respective root causes?
18. How do you track threadpool telemetry—specifically active threads, queue depth, and thread rejection counters for both `write` and `search` threadpools across dedicated data nodes?
19. Shard Relocation & Cluster Rebalancing: How does the cluster orchestrate shard migration upon a node outage, and how do you monitor disk I/O and network transport bandwidth consumption during automated rebalancing?
20. How do you benchmark and analyze indexing latency (milliseconds per document indexed) and search latency to identify indexing degradation before it impacts SOC alert generation?

### Topic 05: Storage Saturation & Disk Watermark Monitoring

21. In indexer cluster storage monitoring, explain the architectural triggers and internal behaviors of the three critical disk watermarks: Low Disk Watermark (85%), High Disk Watermark (90%), and Flood Stage Watermark (95%).
22. When an indexer hits the 95% flood-stage threshold, it automatically enforces a global read-only block (`read_only_allow_delete`), halting all new log ingestion—how do you engineer multi-tiered early-warning alerts (e.g., at 75% and 80%) to proactively prevent this disaster?
23. Disk I/O Saturation & Controller Bottlenecks: How do you correlate system `iostat` metrics (specifically `%util`, `await`, and `svctm`) to diagnose underlying storage controller saturation before filesystem write failures occur?
24. Runway Capacity Forecasting: How do you mathematically calculate daily ingestion growth trends (e.g., an average ingestion rate of 20 GB/day) to project remaining cluster operational runway in days?
25. Inode & File Descriptor Exhaustion: How do you monitor inode consumption (`df -i`) and open file descriptors (`lsof`, `/proc/sys/fs/file-nr`) to prevent catastrophic indexing failures where disk space is available but inode tables are exhausted?

### Topic 06: JVM Runtime, Memory Leaks & Garbage Collection

26. Why must JVM Heap Memory utilization for Java-based indexers (Elasticsearch / OpenSearch) be tuned and capped between 50% and 75% of physical RAM, and what operational hazards arise when heap usage breaches 75%?
27. Garbage Collection (GC) Telemetry: How do you monitor Young Generation (Eden/Survivor) and Old Generation (Tenured) GC frequency, and how do you alert on extended "Stop-the-World" pause durations that trigger node disconnection?
28. Circuit Breaker Telemetry: How do internal memory circuit breakers (e.g., `parent`, `fielddata`, `request`, and `in_flight_requests`) protect indexers from out-of-memory (OOM) crashes during heavy aggregations, and how do you track tripped breaker metrics?
29. Memory Leak Diagnosis: If JVM heap usage exhibits a persistent upward baseline drift despite repeated garbage collection cycles (Memory Sawtooth Degradation), what diagnostic procedure and heap dump analysis do you conduct?
30. Swap Memory Monitoring: Why must swap be permanently disabled on indexer nodes (`vm.swappiness = 1` or `swapoff -a`), and how do you configure critical alerts that fire instantly if the kernel initiates memory paging to swap?

### Topic 07: Search Performance & Heavy Query Monitoring

31. How do you configure and analyze "Slow Search Logs" to identify specific users, dashboards, or SOC analysts running unindexed or expensive wildcard queries that degrade cluster performance?
32. What structured methodology do you employ to establish query execution time thresholds (e.g., flagging queries executing longer than 10 seconds) and generate automated slow-query notifications?
33. Real-Time Scheduled Detection Rule Auditing: If a scheduled detection rule's search execution time exceeds its scheduled run interval, how do you detect this operational overlap before it starves system resources?
34. Dashboard Refresh Governance: How do you audit and throttle aggressive auto-refresh intervals (e.g., uncontrolled 3-second refreshes on broad historical indices) across SOC visualization displays?
35. What telemetry and error-tracking mechanisms do you establish to monitor search queue rejections and query execution exceptions (`SearchPhaseExecutionException`, `EsRejectedExecutionException`)?

### Topic 08: Out-of-Band Telemetry Integration (Prometheus & Grafana)

36. How do you architect an independent, out-of-band monitoring and observability pipeline utilizing Prometheus and Node Exporter to monitor the SIEM infrastructure without circular dependencies?
37. How do you deploy and configure dedicated exporters (e.g., Elasticsearch Exporter, Logstash Exporter) to scrape internal cluster health metrics, shard counts, and pipeline statistics into Prometheus?
38. How do you design an enterprise-grade "SIEM Mission Control & NOC Dashboard" in Grafana to visualize host CPU/RAM, cluster shard distribution, pipeline EPS, and queue depths in real time?
39. How do you integrate Prometheus Alertmanager with PagerDuty, Opsgenie, or corporate messaging platforms to establish automated escalation paths for critical infrastructure failures?
40. What storage sizing models and metric retention policies do you define for Prometheus time-series data to retain long-term historical performance baselines without exhausting monitoring storage?

### Topic 09: Automated Self-Healing & Remediation Playbooks

41. Automated Service Resilience: How do you implement systemd watchdog directives (`Restart=on-failure`, `RestartSec=5s`, `WatchdogSec=30s`) to ensure immediate automated restarts when log collector or agent daemons experience unhandled crashes?
42. Shard Allocation Remediation: What automated remediation workflows do you execute to trigger shard reallocation retries (`_cluster/reroute`) when transient network partitions leave shards in an unassigned state?
43. Automated Storage Protection: How do you configure emergency automated playbooks that trigger index lifecycle policy rollbacks or purge the oldest cold/frozen indices when disk utilization breaches 90%?
44. Task Termination Automation: How do you author automated scripts that poll active cluster tasks and issue cancellations (`POST /_tasks/<task_id>/_cancel`) when runaway queries or unthrottled aggregations saturate node resources?
45. How do you ensure that every automated self-healing action, daemon restart, and index cleanup event is immutably recorded in administrative audit logs for post-incident review?

### Topic 10: Operational Reviews, Capacity Trends & Sign-Off

46. How do you automate the generation of weekly "SIEM Platform Health & Performance Summary Reports" detailing uptime SLAs, EPS averages, peak load metrics, and incident counts for SOC and IT leadership?
47. Capacity Trend Modeling: How do you extrapolate historical 3-month data ingestion trends and EPS projections to justify hardware, compute node, and storage capacity procurements for the upcoming 6 to 12 months?
48. Enterprise SLA Accounting: What mathematical formulas do you apply to measure and prove monthly SIEM platform availability (e.g., 99.98% uptime) and end-to-end event ingestion delivery reliability?
49. Chaos Engineering & Simulation SOP: What standard operating procedure do you execute each quarter to validate the reliability of self-monitoring dashboards and alerting workflows via simulated node and pipeline failure drills?
50. How do you compile and present the complete "SIEM Self-Monitoring & Platform Operational Readiness Sign-Off" package to an Enterprise Director of Infrastructure or CISO for formal operational acceptance?

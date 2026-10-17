# 04 — Infrastructure Sizing

## SIEM Engineer Interview Questions

### Topic 01: EPS Estimation & Workload Characterization

1. For an enterprise with 500 endpoints (400 Windows workstations, 50 Linux servers, 30 Windows servers, 20 network devices), how do you calculate the baseline Average EPS and Peak EPS?
2. How do you measure the EPS variance between normal business hours and off-peak hours, and what scientific methodology determines the peak multiplier (e.g., 2x or 3x)?
3. During a security incident or network vulnerability scan targeting firewalls or Domain Controllers, EPS can surge 5x to 10x above normal; how do you incorporate these event bursts into sizing calculations?
4. In the absence of pre-existing log monitoring telemetry, what methodology do you employ to establish baseline EPS estimates for a greenfield environment?
5. What preliminary average EPS figures would you assume per device category (e.g., Domain Controller, Database Server, Web Server, standard user laptop)?

### Topic 02: Daily Ingestion Volume & Throughput Calculation

6. What is the typical average event size (in bytes) across Windows Event Logs, Syslog, and JSON application logs, and how do you calculate total daily ingestion volume (GB/day)?
7. In an environment experiencing 2,500 Peak EPS during business hours and 500 EPS during off-peak hours, what is the total volume of raw logs generated over a 24-hour cycle?
8. If the endpoint count remains unchanged but audit policies are expanded (e.g., enabling Sysmon or Process Creation Event ID 4688), causing log volume to triple, how do you re-evaluate ingestion capacity?
9. What is the mathematical formula for converting event throughput into megabytes per second (MB/s), and how does this metric guide network interface and pipeline buffer sizing?
10. How do you account for agent-level pre-filtering and noisy event suppression when projecting net ingested log volume?

### Topic 03: Compute Sizing: CPU Cores & Processing Capacity

11. How do you determine the required CPU cores (vCPUs) for the SIEM Manager/Processing Engine and Indexer/Search nodes?
12. How many CPU cores are typically required per 1,000 EPS to handle regex decoding, log parsing, rule correlation, and full-text indexing?
13. What percentage of CPU headroom (safety margin) should be maintained on indexer nodes to prevent CPU saturation during concurrent real-time detection and scheduled queries?
14. How do CPU hyperthreading, context switching, and virtualization overhead (e.g., vCPU overcommit penalties) affect compute sizing in single-node versus multi-node clusters?
15. How do you isolate core allocation between query execution and ingest processing so heavy analytical searches do not throttle ingestion throughput?

### Topic 04: Memory (RAM) Allocation & JVM/Heap Optimization

16. For 500 endpoints and 50 GB/day ingestion, how do you size the total physical RAM across SIEM Manager and Indexer servers?
17. In Java-based indexers (Elasticsearch / OpenSearch), why is the "Golden Rule" of JVM heap allocation (50% of system RAM, capped at 31–32 GB) strictly observed, and why is the remaining 50% critical for OS filesystem cache?
18. How does insufficient memory for in-memory buffers, write queues, and field data cache trigger Out-Of-Memory (OOM) crashes, and how is this mitigated during sizing?
19. How do you size memory footprints for SIEM dashboards and real-time alerting engines separately from indexing nodes?
20. Why does memory swapping severely degrade indexer performance, and how do OS-level configurations (`vm.swappiness=1` or `bootstrap.memory_lock: true`) influence memory sizing?

### Topic 05: Storage Capacity & Data Lifecycle Calculations

21. For 50 GB daily log ingestion with a 90-day searchable retention policy, what is the total raw disk storage requirement, accounting for indexing overhead, replica shards, and OS safety reserves?
22. How do you factor compression ratios (e.g., 1.5:1 or 2:1) and inverted index overhead (typically a 10% to 30% expansion) into storage calculations?
23. When adding 1 replica shard per primary shard (Primary + 1 Replica) for high availability, how does this double storage requirements, and how is it factored into total capacity?
24. How do you calculate disk watermark safety thresholds (e.g., 20% free-space buffer) and OS filesystem metadata overhead to prevent indexer locking and disk exhaustion?
25. After completing 90 days of online searchable retention, how do you size cold or object storage (NFS / AWS S3 / Azure Blob) for the remaining 275 days to fulfill a 1-year compliance mandate?

### Topic 06: Disk Performance, IOPS & Storage Tiering

26. Why is storage throughput and IOPS (Input/Output Operations Per Second) far more critical than raw disk capacity for SIEM indexing nodes?
27. What are the minimum Write IOPS required to sustain 500 baseline EPS and 2,500 Peak EPS, and what Read IOPS are needed for concurrent heavy search queries?
28. How do you select drive technologies—mechanical HDDs (7.2K/10K RPM), SATA SSDs, and NVMe SSDs—when designing Hot Tiers versus Cold Tiers?
29. How does RAID write penalty (e.g., RAID 0, RAID 10 vs RAID 5/6) impact indexing write performance, and why is RAID 10 recommended for high-ingest SIEM storage?
30. How do you formulate a mathematical cost-optimization model using Hot, Warm, and Cold storage tiering (e.g., 7 days Hot on NVMe, 83 days Warm on SATA SSD, and remainder Cold on HDD/Object Storage)?

### Topic 07: Network Bandwidth & Ingestion Pipeline Sizing

31. How do you calculate the network bandwidth (in Mbps) required to transmit log telemetry from remote branches and internal subnets to the central SIEM collector?
32. What is the bandwidth variance between compressed agent telemetry (typically 70–80% bandwidth reduction) versus raw uncompressed Syslog (UDP/TCP)?
33. When forwarding logs over bandwidth-constrained WAN links (e.g., 10 Mbps or 20 Mbps), how do you incorporate bandwidth throttling and rate-limiting into pipeline sizing?
34. Does the internal cluster interconnect (shard replication, node heartbeat, distributed query traffic) necessitate dedicated 1 Gbps or 10 Gbps network interfaces (NICs)?
35. How do you size network interface ring buffers (RX/TX ring buffers) to prevent packet drops and buffer overflows during peak ingestion bursts?

### Topic 08: Buffer, Queue & Backlog Resiliency Sizing

36. How do you size disk storage for message brokers or queuing layers (e.g., Kafka, Logstash persistent queues, Filebeat spoolers) to buffer incoming logs during indexer maintenance or outages?
37. How do you calculate the buffer storage required to absorb incoming logs without data loss during a full 24-hour SIEM indexer outage?
38. Once the indexer recovers, what drain rate (catch-up throughput) is required to process accumulated backlogs alongside live traffic, and what processing headroom is needed?
39. How do you size local endpoint disk queues (in MB or GB) so agents can retain security events during extended network disconnections?
40. How do you evaluate the reliability and performance trade-offs between memory-based queues versus disk-backed persistent queues?

### Topic 09: Growth Forecasting, Capacity Safety Margins & Headroom

41. If an enterprise anticipates annual business and IT expansion of 20% to 30%, what capacity projection and headroom should be built into a 3-year sizing model?
42. What are the financial and operational trade-offs between procuring 3-year "Day 1" on-premises hardware versus adopting a scale-as-you-grow virtual/cloud strategy?
43. How do you allocate unplanned capacity reserves to absorb sudden 20% log surges resulting from emergency regulatory audits or new enterprise application rollouts?
44. When virtualizing SIEM components (VMware ESXi, Proxmox, Hyper-V), why is resource overcommitment hazardous, and why must dedicated resource reservations (CPU/RAM reservation, thick provisioning) be specified?
45. How do you calculate $N+1$ or $N+2$ cluster redundancy sizing to ensure the cluster sustains full peak workloads even when one or two active nodes fail?

### Topic 10: Sizing Validation, Benchmarking & Stress Testing

46. After completing theoretical sizing calculations, how do you utilize `fio` or similar disk benchmarking tools to validate actual write IOPS and storage throughput prior to production sign-off?
47. How do you benchmark CPU utilization, memory pressure, and ingestion latency on target hardware by pushing synthetic peak EPS using log generators or event replays?
48. What key performance indicators (e.g., indexing latency, CPU load average, disk queue depth, search response time) validate that the sizing model is accurate?
49. If synthetic benchmarking reveals an indexer bottleneck at 50% of target EPS, how do you systematically diagnose whether the limitation stems from CPU saturation, JVM heap misconfiguration, or disk IOPS constraints?
50. How do you compile a comprehensive "SIEM Infrastructure Sizing & Specification Sheet" to justify hardware investments to executive management and infrastructure teams?

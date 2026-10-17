# 15 — EPS and Performance Testing

## SIEM Engineer Interview Questions

### Topic 01: Performance Testing Fundamentals & Workload Profiles

1. What is the fundamental objective of performance testing in an enterprise SIEM deployment, and what are the operational distinctions among stress testing, spike testing, and soak/longevity testing?
2. How do you design and simulate realistic workload profiles to evaluate both baseline steady-state throughput (e.g., 5,000 EPS) and peak burst scenarios (e.g., 25,000 EPS) in a staging environment?
3. Which four core golden metrics (ingestion throughput in EPS, end-to-end latency, host resource utilization, and event drop rate) must be continuously measured during load testing?
4. In the absence of production data, why is it critical to synthesize a realistic log diversity profile (mixing Windows Event Logs, Linux Auditd, Firewall traffic, and JSON payloads) rather than replaying repetitive identical strings?
5. How do you construct mathematical modeling and scaling ratios to extrapolate staging cluster test results to production enterprise deployment scales?

### Topic 02: Synthetic Event Generation & Replay Tooling

6. How do you configure synthetic traffic generators (such as `tcpreplay`, `syslog-ng-perf`, `LogGen`, or custom multi-threaded Python scripts) to generate sustained rates of 10,000 to 50,000 Syslog EPS?
7. What is the procedure for leveraging historical packet captures (PCAPs) from real-world DDoS or attack incidents to replay log telemetry at wire speed or accelerated rates (e.g., $5\times$) into a SIEM collector?
8. How do you programmatically randomize IP addresses, usernames, and dynamic timestamps in synthetic streams to prevent artificially high performance from unrealistic cache hits?
9. How do you conduct endpoint agent load testing by utilizing PowerShell or Windows Event Log generators to emit thousands of events per second on Windows test machines?
10. How do you architect a distributed load-generation cluster across multiple test nodes to ensure that the test harness itself does not become the network or CPU bottleneck?

### Topic 03: Queue Backlog, Buffer Filling & Drain Rate Testing

11. How do you test the resilience of collector in-memory buffers and disk-assisted persistent queues by intentionally pausing or stopping downstream indexer services for 30 minutes?
12. Once the indexer service is restored, how do you measure the queue drain rate across accumulated backlogs of tens of millions of events while simultaneously processing ongoing live traffic?
13. If clearing an ingestion backlog requires $2\times$ or $3\times$ normal processing headroom, how do indexer CPU, disk I/O, and indexing latency respond during peak recovery?
14. How do you execute buffer overflow tests to verify system behavior when persistent queue storage reaches maximum disk capacity (verifying graceful rejection vs. unnotified data dropping)?
15. In architectures incorporating Apache Kafka or RabbitMQ, how do you track and visualize consumer lag metrics during simulated ingestion stalls?

### Topic 04: System Hardware & Kernel Bottleneck Profiling

16. How do you utilize Linux diagnostic utilities (`vmstat`, `mpstat`, `pidstat`) to profile context switches, interrupt rates, and `%iowait` during peak ingestion stress tests?
17. When processing sustained workloads of 30,000 EPS, how do you analyze the balance between user-space CPU utilization (`%usr`) and kernel-space CPU consumption (`%sys`)?
18. How do you detect and optimize network driver ring buffer overflows and software interrupt (`ksoftirqd`) CPU saturation under heavy packet ingestion?
19. At what thresholds of average disk write queue length (`avgqu-sz`) and disk service wait time (`await`) do you determine that the storage subsystem is completely I/O saturated?
20. How do you monitor memory paging and swap activity (`si`/`so` in `vmstat`) to immediately detect performance degradation caused by host memory exhaustion during stress tests?

### Topic 05: Indexer Sharding & Threadpool Concurrency Testing

21. How do you evaluate indexer write threadpool queue sizes and rejection rates to identify primary shard concurrency limits across an indexing cluster?
22. How do you experimentally benchmark the practical EPS throughput differences between configuring 1 primary shard versus 5 primary shards on a single index under high write concurrency?
23. What benchmarking methodology do you follow to identify the optimal bulk indexing batch size (e.g., testing 500 vs. 5,000 events, or 5 MB vs. 15 MB batches)?
24. How do you demonstrate that increasing the index refresh interval (e.g., from `1s` to `30s`) yields a 30% to 40% performance gain in bulk ingestion throughput?
25. How do you minimize ingestion stutters and transient throughput drops caused by background Lucene segment merging and translog flush operations?

### Topic 06: Search & Ingestion Concurrency Stress Testing

26. How do you simulate concurrent workload stress when 10+ SOC analysts execute expensive wildcard or regex dashboard queries simultaneously while the cluster ingests peak EPS?
27. How do you prevent search-versus-ingest resource contention when analytical queries consume excessive memory and CPU, thereby starving ingestion threads?
28. When heavy dashboard refreshes cause query latency to inflate from 1 second to 30 seconds, what profiling techniques pinpoint the exact execution bottleneck?
29. How do you measure the performance degradation on bulk indexing throughput when the real-time detection alerting scheduler executes hundreds of recurring correlation queries every minute?
30. How do you determine search queue saturation limits and prevent query timeouts or execution failures (`SearchPhaseExecutionException`) during high concurrency?

### Topic 07: Long-Duration Soak / Longevity Testing (72 Hours)

31. What is the engineering necessity of executing continuous 72-hour soak tests at sustained target workloads (e.g., 10,000 EPS)?
32. How does longevity testing uncover slow memory leaks (heap creep), buffer bloat, and file descriptor leaks that are undetectable during short burst tests?
33. How do you monitor Java Virtual Machine (JVM) garbage collection metrics (specifically G1GC or CMS stop-the-world pause durations and frequency) over multi-day soak tests?
34. How do you verify whether continuous write operations over 72 hours cause filesystem fragmentation, Lucene segment bloat, or storage degradation over time?
35. How do you assess cluster self-healing and stability during a soak test when a node experiences simulated network flickering or transient packet drops?

### Topic 08: Drop Resistance, Backpressure & Network Saturation

36. When an unexpected traffic surge occurs (e.g., bursting from 5,000 to 25,000 EPS), how do you accurately quantify the exact percentage of packet or event drops across the entire pipeline?
37. How do you demonstrate in testing that network interface bandwidth saturation (e.g., 950 Mbps on a 1 Gbps NIC) is the root cause of packet drops, and how is this resolved?
38. How does upstream backpressure signaling propagate through log shippers and intermediate collectors, and how do shippers throttle host log collection during downstream congestion?
39. How do you benchmark drop rates in connectionless UDP Syslog (Port 514) versus connection stalls and TCP reset behaviors in TCP Syslog (Port 514/601) under identical saturation conditions?
40. How do you test whether stateful network firewalls or load balancers exhaust maximum concurrent session tables under heavy syslog traffic, leading to dropped telemetry?

### Topic 09: Performance Tuning & Remediation Playbooks

41. If performance benchmarks reveal that the indexer node becomes unstable at 60% of the target EPS requirement, which architectural parameters should be tuned to double ingestion throughput?
42. What are the best-practice guidelines for tuning Linux kernel TCP stack parameters (`tcp_rmem`, `tcp_wmem`, `somaxconn`) and expanding daemon worker thread pools?
43. How do you tune Linux virtual memory management (`vm.dirty_ratio`, `vm.dirty_background_ratio`) and page caches to optimize I/O-bound indexing servers?
44. What performance gains are achieved when replacing un-optimized regex filter chains with lightweight token dissectors in log parsing pipelines under benchmark testing?
45. Why is the scientific principle of "One Change at a Time" essential during performance optimization, and how do you document test iterations?

### Topic 10: Performance Certification & Sign-Off Report

46. How do you compile a professional "SIEM Load & Stress Test Certification Report" for executive and technical leadership?
47. How do you define and document the platform's "Maximum Sustained EPS", "Maximum Peak Burst EPS", and "Safe Operating Envelope" in the final certification deliverable?
48. How do you visualize resource consumption baselines, queue backlogs, and latency curves across an executive performance dashboard?
49. What data-driven capacity thresholds (e.g., sustained CPU > 75% for 15 minutes or queue depth exceeding 1 hour) should be recommended to trigger horizontal cluster scaling?
50. What is the formal acceptance and sign-off procedure for presenting platform stress-test evidence to a Global CISO or Head of Infrastructure?

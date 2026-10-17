# 22 — HA and Failover Design

## SIEM Engineer Interview Questions

### Topic 01: High Availability Principles & SLA Architecture

1. Why is High Availability (HA) mission-critical for an enterprise SIEM, and what are the architectural complexities and infrastructure cost differentials between achieving 99.9% uptime (8.7 hours downtime/year) versus 99.99% uptime (52 minutes downtime/year)?
2. Single Point of Failure (SPOF) Identification: At what specific layers of an end-to-end SIEM data pipeline (endpoints/agents, network interconnects, load balancers, dedicated syslog collectors, manager cluster, distributed indexers, backend storage) can SPOFs exist?
3. Recovery Time Objective (RTO) and Recovery Point Objective (RPO): How do targeted RTO (how quickly the SIEM restores operations) and RPO (maximum acceptable data loss window during an outage) dictate the underlying HA architecture?
4. Active-Passive (Standby) versus Active-Active (Multi-Master) architectures: What are their operational advantages, clustering complexities, and state-synchronization challenges?
5. How do you mitigate cross-datacenter and inter-region network latency constraints in Cloud Multi-Availability Zone (Multi-AZ) and on-premises Dual-Datacenter active HA deployments?

### Topic 02: Load Balancing & Traffic Distribution

6. How do you configure load-balancing solutions—such as HAProxy, Nginx, or enterprise hardware ADCs (F5 BIG-IP)—to distribute high-volume log ingestion across backend SIEM nodes?
7. Load Balancing Algorithm Selection: Round Robin, Least Connections, or Source IP Hash—which algorithm is optimal for raw Syslog streams versus TLS-encrypted agent traffic, and why?
8. Virtual IP (VIP) & Failover Management: How do you configure `Keepalived` with Virtual Router Redundancy Protocol (VRRP) to guarantee sub-second VIP failover between active and standby load balancer appliances?
9. Layer 4 (TCP/UDP Transport Level) versus Layer 7 (HTTP/HTTPS Application Level) load balancing: What are their architectural differences, and which SIEM ingestion components should utilize each mode?
10. Health Check Engineering: How do you configure passive and active health check probes on load balancers to detect unresponsive backend SIEM nodes and reroute traffic within milliseconds?

### Topic 03: SIEM Manager Clustering & Engine HA

11. In a multi-node SIEM Manager cluster (e.g., Wazuh Cluster), how are architectural responsibilities segregated between the Master Node and Worker Nodes?
12. How does the internal cluster synchronization daemon maintain real-time consistency of configuration states (`cluster.json`, custom rules, decoders, CDB lists, agent cryptographic keys) across all cluster nodes?
13. If the Master Node experiences an unexpected outage, how do Worker Nodes autonomously sustain agent TLS handshakes, event decoding, and rule evaluation without packet loss?
14. Automatic Master Failover (Master Election): What are the configuration parameters, split-brain considerations, and architectural limitations of automatically promoting a Worker Node to Master?
15. How do you tune heartbeat intervals and timeout thresholds between cluster nodes to prevent transient network micro-bursts or link flapping from triggering spurious failover events?

### Topic 04: SIEM Indexer Clustering & Shard Replication

16. In a 3-node or 5-node OpenSearch/Elasticsearch indexer cluster, how does the distributed architecture manage Primary Shard and Replica Shard allocation and routing?
17. If a data node suffers an unrecoverable hardware failure or power outage, how does the cluster automatically promote replica shards within seconds to guarantee zero data loss and uninterrupted searchability?
18. Planned Node Maintenance: How do you leverage `cluster.routing.allocation.enable: "primaries"` to suppress unnecessary, I/O-intensive shard rebalancing storms during planned server reboots?
19. Shard Allocation Awareness (Rack & Zone Awareness): How do you enforce allocation attributes to guarantee that primary shards and their corresponding replicas are never co-located on the same physical server rack or cloud availability zone?
20. Cluster State Recovery: Following a catastrophic datacenter power restoration after an unclean shutdown, how does the cluster achieve quorum and execute master election and shard recovery sequences?

### Topic 05: Dedicated Log Collector HA & Anycast Routing

21. How do you architect High Availability for dedicated syslog collectors ingesting unbuffered UDP and TCP streams from firewalls, routers, and switches?
22. BGP Anycast Routing: How do you configure BGP Anycast to advertise a single virtual IP across multiple syslog collector appliances, enabling autonomous, edge-routed traffic distribution and fault tolerance?
23. Mitigating UDP Packet Loss: Because raw UDP syslog lacks transport-layer retransmissions, how do you minimize packet loss during collector failover using VRRP/VIP clustering and socket buffer tuning?
24. Architectural Trade-offs: How do you balance the operational trade-offs between absolute event redundancy (dual-streaming logs to two independent collectors) versus avoiding index duplication?
25. How do you engineer resilient, dual-hop encrypted tunnels (e.g., WireGuard, IPsec, or TLS forwarders) between remote branch office collectors and the centralized enterprise datacenter?

### Topic 06: Distributed Message Broker HA (Kafka Clustering)

26. In high-throughput enterprise SIEM architectures, how does an Apache Kafka cluster function as an elastic shock-absorber, decoupled queue, and highly available ingestion buffer?
27. How do you configure topic replication factors (`replication.factor = 3`) and producer acknowledgments (`acks = all`, `min.insync.replicas = 2`) to mathematically guarantee zero data loss during broker crashes?
28. Partition Leader Election: When an active Kafka broker fails, how does the cluster automatically execute partition leader re-election and consumer group rebalancing in the background?
29. Quorum Sizing & Metadata Protection: How do you size and architect the voting controller quorum (using KRaft consensus or dedicated ZooKeeper nodes) to ensure cluster metadata availability?
30. Storage Resiliency: Why is configuring hardware RAID 10 and NVMe storage tiering on Kafka broker nodes necessary to survive physical drive failures without broker degradation?

### Topic 07: Network & Hardware Redundancy (Bonding & Dual-Homed)

31. How do you configure Linux network bonding (NIC Teaming / Bonding Mode 4: 802.3ad LACP) across enterprise SIEM servers to provide physical NIC redundancy and link aggregation?
32. Dual Top-of-Rack (ToR) Switches & Redundant Power (Dual PSU): How do you cross-connect enterprise SIEM server hardware to survive the sudden failure of an entire rack switch or power distribution unit (PDU)?
33. Multi-Chassis Link Aggregation (MLAG) & Spanning Tree (STP): How do you configure upstream switch fabric interconnects to eliminate STP port blocking and forwarding loops across redundant paths?
34. Redundant Cloud Networking: How do you architect dual-homed connectivity in AWS/Azure using multiple Elastic Network Interfaces (ENIs), Transit Gateways, and multi-region direct connects?
35. Enterprise Hardware RAID: How do dual battery-backed write cache (BBU) RAID controllers configured with mirrored/striped arrays maintain uninterrupted I/O throughput during physical disk failures?

### Topic 08: Failover Testing & Chaos Engineering

36. Chaos Engineering for SIEM: How do you validate the real-world resilience of your HA architecture by executing physical pull-the-plug tests on the Master SIEM node under live, peak ingestion load?
37. Failover Duration Measurement: How do you script and automate the measurement of end-to-end failover latency (the exact time elapsed between primary node termination and standby traffic absorption)?
38. Network Blackhole Simulation: How do you use host-based firewall rules to simulate intermittent network partition drops on cluster interconnect ports, auditing cluster quorum response and alert dispatching?
39. Dropped Message Auditing: When generating 10,000 EPS through an ingestion pipeline during an active node failover, what mathematical audit methodology determines the exact count of lost or retried events?
40. What standard operating procedure governs the scheduling, execution, observer alignment, and stakeholder sign-off of an enterprise SIEM Disaster Simulation Drill?

### Topic 09: Split-Brain Mitigation & Quorum Enforcement

41. What is Split-Brain Syndrome in distributed systems, and what catastrophic data corruption occurs if a network partition causes isolated cluster segments to independently elect masters and write conflicting data?
42. Quorum Voting Rule ($N/2 + 1$): Why must the count of master-eligible nodes in a high-availability cluster always be an odd number (3, 5, or 7), and how does majority voting prevent split-brain states?
43. How do modern distributed consensus protocols (e.g., OpenSearch/Elasticsearch Raft Consensus and `cluster.initial_master_nodes`) prevent split-brain scenarios during initial bootstrap and network partitions?
44. Minority Partition Isolation: How does an isolated cluster partition that falls below quorum automatically transition into a read-only or offline state to prevent divergent data mutations?
45. Automatic Healing & Shard Reconciliation: How does the distributed storage engine reconcile divergent transaction logs and restore cluster health once the severed network partition recovers?

### Topic 10: HA Documentation, ADR & Architecture Sign-Off

46. What essential diagrams, failure state transition matrices, and recovery timeline models must be compiled within a formal Enterprise SIEM High Availability Architecture Document?
47. Architecture Decision Records (ADR): How do you formulate an ADR documenting the technical, operational, and financial trade-offs of choosing an Active-Active versus an Active-Passive clustering model?
48. How do you author a comprehensive, actionable "Failover & Fallback Operational Runbook" for SOC operations and infrastructure engineering teams?
49. What verifiable documentation and empirical failover benchmarks do you present to external regulatory auditors to prove continuous compliance with 99.99% uptime mandates?
50. As an Enterprise SIEM Architect, what rigorous testing evidence and SLA validation artifacts do you deliver to the Chief Technology Officer (CTO) to secure formal production sign-off?

# 26 — Storage and Retention Management

## SIEM Engineer Interview Questions

### Topic 01: Storage Tiering Architecture & Economics

1. Why is a multi-tier storage architecture (Hot, Warm, Cold, Frozen, and Archive tiers) essential in an enterprise SIEM, and why is retaining all historical telemetry on identical premium storage an unacceptable financial waste?
2. What are the data lifecycle movement criteria between the Hot tier (high-throughput NVMe SSD for active indexing and real-time analytics), Warm tier (cost-effective SATA SSD for read-only querying), and Cold/Frozen tier (high-capacity HDD or object storage)?
3. Total Cost of Ownership (TCO) Optimization: How do you leverage tier pricing differentials (e.g., NVMe at \$0.20/GB vs Cloud Object Storage at \$0.02/GB) to achieve up to a 70% storage cost reduction for 1-year enterprise log retention?
4. What are the architectural best practices for designing on-premises datacenter physical storage tiering across SAN, NAS, and Direct-Attached Storage (DAS)?
5. Query Latency Trade-Offs: When historical log searches transition to slower storage tiers (e.g., search latency increasing from 1 second to 5–10 seconds), how do you architect caching and communicate SLA expectations to the SOC team?

### Topic 02: Index Lifecycle Management (ILM / ISM) Engineering

6. How do you design, script, and implement Index State Management (ISM) policies in OpenSearch or Index Lifecycle Management (ILM) in Elasticsearch via declarative JSON policies?
7. In a production ISM/ILM policy, how do you define automated phase transitions: Hot to Warm (e.g., after 30 days), Warm to Cold (e.g., after 90 days), and Cold to Deletion (e.g., after 365 days)?
8. Automated Index Rollover: What configuration parameters trigger an automated index rollover when a primary shard reaches 50 GB or an index age exceeds 30 days?
9. Post-Rollover Optimization: How do you configure automated policy actions to transition aged indices to read-only (`read_only: true`) and reduce replica shard counts to reclaim cluster capacity?
10. Failure Handling: How do you monitor, debug, and configure automated alerting for ISM/ILM policy execution errors (e.g., failed index migrations, unassigned shard relocation errors, or resource locks)?

### Topic 03: Shard Sizing, Allocation & Over-Sharding Prevention

11. Why is the recommended target primary shard size typically maintained between 30 GB and 50 GB, and what operational hazards arise when shards are either undersized (<100 MB) or oversized (>200 GB)?
12. The Over-Sharding Disaster: Why does accumulating hundreds of thousands of small shards exhaust JVM heap memory, degrade cluster state synchronization, and trigger cluster-wide freezes?
13. Heap-to-Shard Ratio: How do you enforce and monitor the golden architectural rule of maintaining no more than 20 active shards per 1 GB of JVM heap memory across data nodes?
14. Shard Allocation Filtering: How do you leverage custom node attributes (`node.attr.box_type: hot` vs `warm`) and index routing settings to pin indices strictly to designated hardware tiers?
15. Shard Consolidation: Can primary shard counts be modified post-creation? How do you utilize the Shrink API (`_shrink`) to consolidate multiple primary shards into a single optimized primary shard?

### Topic 04: Storage Compression & Segment Optimization

16. Storage Compression Codecs: What are the performance overhead and storage efficiency trade-offs between default `LZ4` compression and high-density codecs (`best_compression` / Zstandard) at the indexer layer?
17. Segment Optimization via Force Merge: How does executing a force merge with `max_num_segments=1` on read-only Warm or Cold indices purge deleted Lucene tombstones, reclaim disk space, and accelerate search queries?
18. Why must Force Merge never be executed on actively indexing Hot indices, and why should it be strictly scheduled during off-peak maintenance windows on read-only indices?
19. Index Mapping Optimization: How do you reduce indexing overhead and storage bloat without disabling `_source` by setting `index: false` or `doc_values: false` on non-searchable or non-aggregatable fields?
20. Ingestion Payload Hygiene: What filtering and deduplication strategies at the collection/parsing layer allow you to eliminate redundant fields and noisy events, reducing daily storage footprint by 20% to 30%?

### Topic 05: Compliance Retention & Legal Hold Engineering

21. How do you design an enterprise log retention architecture that satisfies strict regulatory compliance frameworks, such as PCI-DSS (90 days active, 1 year total), HIPAA (6 years), ISO 27001, and local statutory laws?
22. Architectural Segregation: How do you structure data flow between Online Searchable Retention (Active/Searchable hot/warm indices, e.g., 90 days) and Long-Term Cold/Offline Archival Retention (remaining 9 months to multiple years)?
23. Legal Hold / Litigation Freeze: How do you implement immutable locks on specific indices or historical datasets to prevent automated ILM deletion workflows from purging records during ongoing investigations?
24. Auditability of Retention Execution: How do you generate cryptographically verifiable audit trails documenting the exact ingestion timestamp and authorized policy-driven deletion timestamp for every index?
25. Reconciling Conflicting Regulatory Mandates: When one regulation mandates data disposal after 90 days (e.g., GDPR minimization) while another demands retention for 1 year (e.g., financial audit), how do you architect compliance resolution?

### Topic 06: Searchable Snapshots & Cloud Object Storage

26. Searchable Snapshots / Remote-Backed Storage: What is the underlying architecture, and how does it allow direct querying against cold/frozen datasets stored directly in low-cost cloud object storage (AWS S3, Azure Blob)?
27. Local Shared Cache: How does the local NVMe caching tier function to accelerate search queries on remote object-backed snapshots by caching frequently accessed index segments?
28. Frozen Tier Economics: How does offloading primary and replica shard storage to cloud object storage via the Frozen Tier reduce physical compute and disk costs by up to 90%?
29. Architectural Comparison: What is the operational distinction between retaining snapshot backups in cloud object storage for disaster recovery versus mounting them as searchable snapshots for historical threat hunting?
30. Network Egress and Bandwidth Governance: How do you manage and forecast cloud egress network bandwidth and costs when SOC analysts query large historical datasets residing in cloud object storage?

### Topic 07: Log Purging, Defensible Disposal & Sanitization

31. How does the automated deletion phase (`action: { delete: {} }`) within ISM/ILM systematically purge expired indices without administrative intervention?
32. Defensible Disposal: What documentation and cryptographic records are required to prove to external auditors that expired logs were sanitized according to pre-approved governance policies rather than accidental loss?
33. GDPR "Right to be Forgotten": When served with a legal deletion request for an individual's personal data, what is the architectural impact and performance penalty of executing selective record deletion (`_delete_by_query`) across immutable log indices?
34. Secure Storage Sanitization: What procedures (e.g., Cryptographic Erasure, NIST SP 800-88, DoD 5220.22-M) are enforced when decommissioning physical disks or storage volumes holding enterprise log data?
35. Safety Guardrails Against Accidental Destruction: How do you enforce safety configurations (such as setting `action.destructive_requires_name: true`) to prevent destructive wildcard deletion commands (`DELETE /*`)?

### Topic 08: Storage Runway Capacity Forecasting

36. Storage Runway Capacity Forecasting: What mathematical methodology and formulas do you use to calculate the exact calendar exhaustion date of cluster storage based on daily ingestion rates and retention policies?
37. Explain in detail the components of the enterprise storage sizing formula:
$$\text{Total Storage Needed} = \frac{\text{Daily Ingestion GB} \times \text{Retention Days} \times (1 + \text{Indexing Overhead}) \times (1 + \text{Replicas})}{1 - \text{Watermark Margin}}$$
38. Predictive Capacity Alerting: How do you configure early-warning alerts (e.g., triggering a P2 notification when "Projected Runway < 14 Days") when sudden ingestion spikes accelerate disk consumption?
39. Sizing Modeling for Annual CapEx: How do you model linear storage projection versus exponential business growth when presenting multi-year infrastructure budgets?
40. Virtualization Hazards: Why is Thin Provisioning considered an unacceptable operational risk for high-throughput SIEM data stores, and why must Thick Provisioning Eager Zeroed be mandated?

### Topic 09: Emergency Disk-Full Recovery Playbooks

41. Emergency Cluster Unlocking: When storage crosses 95% and the cluster enforces a cluster-wide read-only lock (`read_only_allow_delete: true`), what is the precise operational procedure to restore read-write operations without data loss?
42. Step-by-Step Emergency Remediation Commands: How do you purge temporary heap dumps and logs, drop non-critical aging indices, and clear the global `read_only_allow_delete` index block via REST API?
43. Operating System Recovery on 100% Disk Exhaustion: When a filesystem reaches 100% capacity causing the indexer daemon to crash and fail to start, what OS-level remediation and safe file pruning techniques do you employ?
44. Shard Corruption Diagnosis: If Lucene indices become corrupted due to hard disk crashes or sudden power loss during disk-full states, how do you identify corrupted shards and execute recovery?
45. Post-Incident Review & Root Cause Analysis (RCA): What structure and remediation action items must be included in a formal engineering RCA following a production disk-full outage?

### Topic 10: Storage Governance, Auditability & Sign-Off

46. How do you construct a comprehensive enterprise "Log Retention & Storage Lifecycle Matrix" mapping every telemetry source to its business criticality, regulatory mandate, and tiering lifecycle?
47. How do you implement automated audit reporting mechanisms to continuously prove retention compliance and disposal integrity to external cybersecurity and financial auditors?
48. Change Management & Governance: What formal review, risk acceptance, and approval workflow is required before modifying production retention windows (e.g., reducing active retention from 90 to 60 days)?
49. FinOps & Storage ROI: How do you author a quantifiable "FinOps & Storage ROI Report" demonstrating multi-thousand-dollar annual cost savings achieved through lifecycle tiering and segment compression?
50. What is the formal operational sign-off procedure to present the complete SIEM Storage and Retention Lifecycle framework to an Enterprise Chief Data Officer (CDO) or Principal Infrastructure Architect?

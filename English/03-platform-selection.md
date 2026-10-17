# 03 — Platform Selection

## SIEM Engineer Interview Questions

### Topic 01: Core Platform Comparison & Evaluation Criteria

1. What are the architectural differences among Wazuh, Splunk Enterprise Security, Elastic Security (ELK), and Microsoft Sentinel, and how would you evaluate their core strengths and limitations?
2. For an organization with 500 endpoints, an on-premises data center, and a constrained budget, which platform among Wazuh, Elastic, and Splunk is most suitable, and why?
3. If an enterprise operates entirely within Microsoft Azure or a hybrid cloud ecosystem, what are the primary advantages and potential drawbacks of choosing Microsoft Sentinel?
4. When choosing between an open-source-based SIEM (e.g., Wazuh / OpenSearch) and a commercial enterprise SIEM (e.g., Splunk), what technical and operational factors must you evaluate?
5. If security leadership has a preconceived bias toward a specific brand or vendor, how would you establish an objective, criteria-driven evaluation framework?

### Topic 02: Licensing Models & Total Cost of Ownership (TCO)

6. How do you calculate and compare the disparate licensing models of Splunk (GB/day ingestion or workload-based), Sentinel (GB-ingestion pay-as-you-go), Elastic (compute/RAM resource-based), and Wazuh (open-source / optional commercial support)?
7. When selecting a platform, how do you project the Total Cost of Ownership (TCO)—including software licensing, server hardware, cloud storage, WAN bandwidth, administrative overhead, and consulting services?
8. When ingesting high-volume, verbose log sources (e.g., Firewalls, NetFlow, DNS), which platform licensing model becomes the most cost-prohibitive, and what data-reduction strategies can mitigate this?
9. When using Microsoft Sentinel or cloud-native SIEMs, how do you evaluate the financial risk of ballooning log retention costs for long-term compliance (1 year or more)?
10. Although open-source solutions lack upfront software licensing costs, how do their dedicated engineering overhead, custom development, and infrastructure operational costs compare to commercial enterprise tools?

### Topic 03: Deployment Model & Infrastructure Footprint (On-Prem vs Cloud vs SaaS)

11. How do you evaluate and choose among On-Premises deployment (Wazuh/Splunk/Elastic), Cloud-Hosted (Splunk Cloud/Elastic Cloud), and pure SaaS/Cloud-Native (Microsoft Sentinel)?
12. How do data sovereignty regulations, local compliance mandates, or air-gapped environments impact the feasibility of Cloud SIEM versus On-Premises SIEM?
13. For 500 endpoints generating 50 GB of daily logs, what are the technical and financial justifications for hosting on-premises servers (CPU, RAM, NVMe SSDs) versus utilizing a cloud SIEM platform?
14. When selecting a cloud-native SIEM, how do you evaluate Internet egress bandwidth costs, uplink saturation risks, and outbound network security from on-prem data centers?
15. In a hybrid enterprise environment (spanning on-premises data centers, AWS, and Azure), how do log ingestion latency and network topology influence platform selection?

### Topic 04: Log Ingestion, Agent Architecture & Collector Capabilities

16. How do resource overhead (CPU/RAM footprint) and transport reliability compare among Wazuh Agent, Splunk Universal Forwarder, Elastic Agent/Beats, and Azure Monitor Agent (AMA)?
17. For agentless appliances (e.g., network switches, routers, firewalls), how do you evaluate each platform's native Syslog handling, API integrations, and collector scalability?
18. How do the platforms compare regarding local disk caching, queue buffering, and durability guarantees against data loss during network disconnections?
19. Which platform offers the greatest flexibility when ingesting and parsing unstructured or custom proprietary application logs?
20. How do the platforms differ in their centralized capabilities for pushing agent configurations, managing automated upgrades, and monitoring agent health fleets?

### Topic 05: Search Performance, Query Languages & Indexing

21. How do the learning curves and analytical capabilities compare among Splunk's SPL (Search Processing Language), Sentinel's KQL (Kusto Query Language), Elastic's EQL/ES|QL, and OpenSearch/Wazuh query mechanisms?
22. When performing high-speed historical investigations and real-time threat hunting across billions of events, what are the architectural trade-offs between Schema-on-Read and Schema-on-Write?
23. What are the technical distinctions among Splunk, Elastic, and Wazuh (Indexer) regarding indexing throughput, sharding strategies, and storage compression ratios?
24. Which platform's query language is most powerful and performant when executing complex security investigations involving subqueries, joins, lookups, and time-series aggregations?
25. How do existing SOC analyst technical skill sets and query language adoption curves influence your platform selection?

### Topic 06: Detection Engineering, Rule Ecosystem & Correlation

26. How mature, comprehensive, and actively maintained are the out-of-the-box detection rule ecosystems (e.g., Splunk ES Content Updates, Sentinel Analytic Rules, Elastic Prebuilt Rules, Wazuh Ruleset) across each platform?
27. How do the platforms compare in their ability to build threshold alerts, stateless rules, and complex multi-step time-window stateful correlations (e.g., 3 failed logins followed by a success and immediate privilege escalation within 5 minutes)?
28. What are the advantages and architectural limitations of memory-based real-time stream correlation versus scheduled index-search correlation engines?
29. How easily can each platform's detection rules be mapped to the MITRE ATT&CK framework, and how do they visualize enterprise threat coverage?
30. Which platform provides the most robust API and automation ecosystem for implementing Detection-as-Code (Git, CI/CD testing pipelines, versioning, and automated rollback)?

### Topic 07: Scalability, Clustering & High Availability

31. How complex and cost-intensive is horizontal clustering (Master/Worker, Cluster Master, Indexer Cluster, Search Head Cluster) when scaling from 500 to 5,000 or 10,000 endpoints across each platform?
32. How do the High Availability (HA) architectures and multi-node automated failover mechanisms function in each platform to eliminate single points of failure (SPOF)?
33. For distributed, multi-region enterprise data centers, how do the platforms support federated search across remote indexers from a centralized search head or console?
34. How robust are the Multi-Tenancy and Role-Based Access Control (RBAC) mechanisms across platforms for strictly isolating data and dashboards between different business units or clients?
35. During sudden log spikes or massive event bursts (e.g., during active DDoS or ransomware attacks), how do the platforms handle backpressure and resist log drops?

### Topic 08: Integration, Ecosystem & SOAR/Threat Intel Support

36. How extensive and mature are each platform's third-party integration ecosystems (Splunkbase, Elastic Integrations, Sentinel Data Connectors, Wazuh Community)?
37. How effectively do the platforms ingest, parse, and correlate commercial and open-source Threat Intelligence Feeds (MISP, AlienVault OTX, Mandiant, etc.) against real-time logs?
38. How straightforward is bidirectional integration with Security Orchestration, Automation, and Response (SOAR) tools and IT ticketing platforms (Jira, ServiceNow, TheHive)?
39. Does the platform provide native endpoint active response capabilities (e.g., host isolation, process termination, IP blocking, like Wazuh Active Response), or does it rely entirely on external SOAR/EDR tools?
40. How do vendor support SLAs (24/7 mission-critical hotlines, premium engineering support) versus open-source community support impact operational risk during enterprise outages?

### Topic 09: SIEM Migration, Platform Replacement & Co-existence

41. If an enterprise decides to migrate from a legacy SIEM (e.g., legacy QRadar or ArcSight) to a modern platform (e.g., Wazuh or Splunk), how do you design parallel co-existence during the transition phase?
42. During a SIEM migration, how do you decide whether to ingest historical logs (e.g., 9 months of past logs) into the new platform versus retaining them in cold archives and only ingesting new logs going forward?
43. What strategies and automated tooling can be utilized to convert hundreds of legacy detection rules, custom dashboards, and parsers into the syntax of the new SIEM?
44. How do you orchestrate agent migration across 500+ endpoints—deploying the new agent and decommissioning the legacy agent—without causing production system disruptions?
45. What criteria and testing methodology do you use to validate data parity, alert parity, and pipeline performance before formally decommissioning the legacy SIEM?

### Topic 10: Decision Framework, Trade-Offs & Stakeholder Justification

46. What framework and criteria weighting would you use to build a comprehensive "Platform Selection Scoring Matrix" for management, finance, and security stakeholders?
47. When working under strict budgetary constraints while needing rigorous enterprise compliance and endpoint file integrity monitoring, how do you balance feature depth against platform cost?
48. How do you document the technical justification, architectural trade-offs, and rejection reasons for alternative platforms within an Architecture Decision Record (ADR)?
49. What are the top 5 measurable success criteria you would define for a real-world Proof of Concept (POC) prior to final platform procurement?
50. What contingency and exit plans would you formulate to mitigate the risk of vendor lock-in, unforeseen cost escalation, or operational bottlenecks over a 1-to-3-year horizon?

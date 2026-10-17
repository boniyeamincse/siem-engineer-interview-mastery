# 16 — Detection Rule Engineering

## SIEM Engineer Interview Questions

### Topic 01: Threat Modeling & Detection Requirement Analysis

1. How do you translate Cyber Threat Intelligence (CTI), newly disclosed CVEs (e.g., Log4j or Spring4Shell), or Red Team assessment findings into production-ready detection engineering requirements?
2. Data-driven detection (authoring rules based on available log telemetry) versus Threat-driven detection (authoring rules based on adversary tactics, techniques, and procedures)—which approach represents modern industry best practice, and why?
3. Prior to writing correlation detection logic, what systematic methodology do you follow to verify that requisite log sources, Event IDs, and normalized schema fields are actively collected and present in the SIEM?
4. How do you evaluate the durability and operational lifespan of Indicator of Compromise (IOC: IP addresses, domains, file hashes) detection versus Tactics, Techniques, and Procedures (TTP: behavioral patterns) detection in the context of the Pyramid of Pain?
5. What structured checklist or assessment criteria do you use to evaluate feasibility, telemetry fidelity, and Return on Investment (ROI) upon receiving a new detection rule request?

### Topic 02: Detection Logic Paradigms & Rule Types

6. What is the architectural and operational difference between a stateless single-event detection rule (e.g., Mimikatz process execution) and a stateful correlation rule (e.g., 1 successful logon preceded by 10 failed logon attempts within 5 minutes)?
7. When engineering threshold- and frequency-based detection rules (Count Rules), what are the operational risks of using static cut-off numbers (e.g., 10 failures), and why is dynamic statistical baselining preferred?
8. Time-Window Sliding Correlation: If incoming events arrive out-of-order due to network latency or agent queue delays, how does a sliding time window maintain temporal integrity and ensure accurate sequence evaluation?
9. How do you design aggregation- and bucket-based correlation rules (e.g., detecting a single user account authenticating from more than 10 distinct source IP addresses within a rolling 1-hour window)?
10. What are the primary technical challenges and mitigation strategies when configuring statistical anomaly detection or machine learning-driven detection rules (e.g., abnormal outbound data transfer volume outside baseline business hours)?

### Topic 03: Wazuh Rule Engine & XML Syntax Engineering

11. In Wazuh's XML rule architecture, explain the operational significance of rule severity levels (`level="0"` through `16`) in the `<rule id="..." level="...">` tag, and how alerting thresholds and email dispatch triggers function.
12. Explain with concrete XML syntax examples how the `<if_sid>` and `<if_matched_sid>` tags are utilized to construct hierarchical parent-child rule dependency chains based on prior rule triggers.
13. How do you compose a robust brute-force detection rule combining parameters such as `<frequency>`, `<timeframe>`, `<same_source_ip>`, and `<same_user>` to correlate repeated authentication failures from the same origin?
14. What are the best practices and regex syntax constraints for implementing dynamic field pattern matching using the `<field name="...">` tag in Wazuh rule definitions?
15. When deploying custom detection rules in Wazuh, how do you manage rule IDs (e.g., reserving IDs 100000 and above) and directory structures to ensure clean extensions without overriding default system rules or breaking future updates?

### Topic 04: Splunk, Elastic & Sentinel Detection Engineering

16. In Splunk Enterprise Security, why do Data Models and accelerated `tstats` queries execute up to 100 times faster than standard raw `search` commands for Correlation Searches?
17. In Elastic Security, how do you implement process-tree sequence correlation using Event Query Language (EQL) (e.g., `sequence with maxspan=1m [process where ...] [network where ...]`)?
18. In Microsoft Sentinel, how do you author advanced analytics rules using Kusto Query Language (KQL) by performing cross-table `join` operations between diverse tables (e.g., `SigninLogs` and `DeviceProcessEvents`)?
19. How do you evaluate the architectural performance trade-offs between scheduled query analytics rules and Near-Real-Time (NRT) streaming rules in terms of SIEM resource utilization and detection latency?
20. What is the design pattern and architectural flow for integrating contextual lookup tables (e.g., CIDR ranges, Threat Intel blacklists, VIP user lists) into detection query languages across these SIEM platforms?

### Topic 05: Multi-Source & Complex Cross-Data Correlation

21. How do you construct a unified correlation rule that correlates telemetry across three distinct data sources—Next-Gen Firewalls, EDR/Sysmon endpoints, and Active Directory Domain Controllers—to track an end-to-end Cyber Kill Chain?
22. Scenario: "A phishing email attachment is executed (Email Security log) -> Microsoft Word spawns PowerShell (Sysmon Event ID 1) -> immediate outbound C2 network connection to an unknown external IP (Firewall log)"—how would you engineer the correlation logic to detect this exact multi-stage sequence?
23. How do you engineer cross-environment correlation rules linking hybrid cloud and on-premises events (e.g., an active on-premises corporate VPN session paired with concurrent anomalous administrative activity in AWS/Azure cloud console)?
24. When correlating events across disparate telemetry sources with inconsistent primary linking entities (e.g., Hostname vs. IP address vs. sAMAccountName vs. UPN), how do you resolve entity discrepancies to achieve successful correlation?
25. How do you optimize the evaluation time window (`maxspan`, e.g., 5 to 10 minutes) for multi-source correlation rules to prevent excessive memory state retention and exhaustion on search heads/engines?

### Topic 06: Rule Severity, Dynamic Risk Scoring & Prioritization

26. What are the inherent limitations of static severity ratings (Critical, High, Medium, Low), and why is implementing dynamic Risk-Based Alerting (RBA) essential in modern enterprise SOC environments?
27. How does Asset and Identity Priority Weighting (e.g., malware execution on a disposable test VM = Low risk, whereas identical execution on a Tier-0 Domain Controller = Critical risk) influence aggregate alert risk scoring?
28. How do you dynamically adjust alert severity and priority based on user privilege levels (Domain Admin vs. standard domain user) and network zone criticality (PCI-DSS CDE zone vs. Guest Wi-Fi)?
29. How do you configure Risk-Based Alerting (RBA) logic so that instead of alerting SOC analysts on individual low-severity signals, a high-priority incident is generated only when the cumulative risk score for a single entity exceeds 100 within a rolling 24-hour window?
30. What methodologies and threshold tuning strategies do you employ to reduce alert volume and prevent SOC alert fatigue while ensuring zero degradation in threat detection coverage?

### Topic 07: Context Enrichment & Threat Intelligence Lookups

31. What automated mechanisms and architectural pipelines allow an alert to be enriched on-the-fly with threat intelligence reputation data (e.g., MISP, AlienVault OTX, VirusTotal) the moment a detection rule fires?
32. How do you execute real-time or automated Active Directory LDAP lookups during rule evaluation/triaging to inject user department, manager, job title, and employment status into alert metadata?
33. What rule logic and lookup configurations are required to automatically classify external remote IP addresses as "Internal", "Partner Network", or "Public Internet" using internal CIDR subnet mappings?
34. How do you implement local memory caching (e.g., Redis or in-memory hash tables) to prevent synchronous external API/LDAP lookups from throttling the log ingestion pipeline or causing network latency?
35. What is the standard operational procedure for dynamically updating Threat Intel watchlists/threat feeds without requiring SIEM service restarts or manual detection rule reloads?

### Topic 08: Sigma Rule Engineering & Cross-Platform Translation

36. What is the Sigma rule standard, and why is it essential for an enterprise SIEM engineer to author vendor-agnostic detection rules in Sigma format alongside platform-native queries?
37. Explain the core structural components of a Sigma rule file (Metadata, Logsource, Detection Selection/Filter, Condition, Falsepositives, Level) with an example.
38. How do you utilize `pySigma` or the Sigma CLI to automatically compile/transpile generic Sigma rules into target platform syntaxes, including Wazuh XML, Splunk SPL, and Elastic EQL/Lucene?
39. How do you design and maintain custom Sigma pipeline configurations (Mapping YAML) to ensure custom field names in Sigma rules map accurately to your enterprise SIEM's normalized schema?
40. What workflow do you follow to ingest, test, validate, and customize open-source Sigma community rules before safely deploying them into an enterprise production SIEM?

### Topic 09: Rule Performance & Query Optimization

41. What detrimental performance impacts do inefficient detection queries (e.g., leading wildcards `*mimikatz*` or unindexed full-table scans) have on SIEM indexer CPU utilization and search throughput?
42. What mathematical and architectural principles govern query optimization, such as eliminating wildcards, targeting indexed structured fields (`process.name: "mimikatz.exe"`), and restricting search time boundaries?
43. If a scheduled detection rule running on a 1-minute interval has an execution runtime exceeding 60 seconds, how does this induce "Search Head Queue Starvation" or pipeline lag, and how do you resolve it?
44. How do you control the state memory footprint of a stateful correlation engine to prevent Out-Of-Memory (OOM) crashes when tracking billions of stateful events concurrently?
45. How do you leverage SIEM slow-query logs, audit indexes, and search profiler metrics to identify and isolate resource-intensive detection rules across the cluster?

### Topic 10: Rule Documentation, SOC Runbooks & Governance

46. After authoring a production detection rule, how do you construct a comprehensive SOC Triage Runbook (Investigation Playbook) for Tier-1 and Tier-2 analysts?
47. What specific sections should an investigation playbook contain (e.g., attack overview, legitimate business use cases/false positive scenarios, step-by-step investigation procedures, and containment actions like host isolation or account disablement)?
48. What industry-standard framework do you use to maintain a "Detection Rule Specification Sheet" capturing metadata such as rule name, description, author, creation date, MITRE ATT&CK mapping, version history, and testing evidence?
49. How do you manage the detection rule lifecycle, including periodic review, deprecation, and decommissioning when rules lose relevance due to environmental or architectural changes?
50. How do you structure and present a formal business justification, risk-reduction assessment, and performance impact analysis to SecOps management to secure sign-off for a new enterprise detection rule?

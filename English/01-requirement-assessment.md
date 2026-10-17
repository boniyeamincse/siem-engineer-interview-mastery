# 01 — Requirement Assessment

## SIEM Engineer Interview Questions

### Topic 01: Business & Security Requirements

1. In an organization with 500 endpoints, firewalls, VPNs, switches, and critical servers, how would you gather business and security requirements before initiating SIEM deployment?
2. If executive management simply states, "We need a SIEM," how do you translate that broad objective into actionable technical requirements?
3. How do you incorporate the organization's security objectives, operational requirements, and compliance mandates into the SIEM project scope?
4. What measurable acceptance criteria would you define to determine whether the SIEM deployment is successful?
5. What critical technical information must you elicit from the IT, Network, System Administration, and Security Operations teams?

### Topic 02: Asset Inventory & Endpoint Assessment

6. How do you inventory and categorize the organization's 500 endpoints into Windows workstations, Linux workstations, Windows Servers, and Linux Servers?
7. If the organization's asset inventory is incomplete or outdated, how do you construct an accurate, reconciled asset list prior to SIEM deployment?
8. What onboarding requirements must you define for domain-joined endpoints, standalone systems, remote teleworker workstations, virtual machines, and cloud-hosted instances?
9. How do you differentiate logging requirements for critical servers, Domain Controllers, database servers, and general user workstations?
10. How do you reconcile the authoritative asset inventory with the active agents and log sources onboarded into the SIEM?

### Topic 03: Log Source Identification

11. How do you determine which specific logs must be ingested across Windows, Linux, Active Directory, DNS, DHCP, firewalls, VPNs, switches, IDS/IPS, and applications?
12. If a critical internal application lacks vendor logging documentation, how do you discover, assess, and define its logging requirements?
13. How do you determine which log sources require agent-based collection, Syslog forwarding, or API-based integration?
14. If technical or budgetary constraints prevent onboarding all enterprise log sources simultaneously, how do you prioritize sources for ingestion?
15. How do you document each log source's event types, expected volume, collection mechanism, transport protocol, and system ownership?

### Topic 04: Critical Assets & Business Prioritization

16. How do you identify which of the organization's 500 endpoints constitute business-critical assets?
17. How do you establish logging tiers and ingestion priorities among Domain Controllers, Active Directory, VPN gateways, firewalls, database servers, and standard workstations?
18. How do you assess whether business-critical assets require enhanced audit logging (e.g., granular file integrity monitoring or command-line auditing)?
19. If a critical server cannot emit required security telemetry due to legacy constraints, how do you document and escalate this requirement gap?
20. How do you define the relationship between asset criticality, potential business impact, and log ingestion priority?

### Topic 05: Log Volume & EPS Requirements

21. How do you estimate the expected EPS (Events Per Second) and daily log volume (GB/day) for each log source prior to deployment?
22. In the absence of historical log metrics, how do you calculate initial baseline volume and throughput estimates for infrastructure sizing?
23. How do you measure and account for the variance between normal business hours, off-peak activity, and sudden event bursts?
24. If the endpoint count remains constant but ingested log volume surges dramatically, how do you investigate root causes and evaluate the infrastructure impact?
25. How do you formally document Average EPS, Peak EPS, daily ingestion volume (GB/day), and future growth projections during requirement assessment?

### Topic 06: Retention & Compliance Requirements

26. How do you capture and translate organizational retention mandates—such as 30-day, 90-day, 180-day, or 1-year retention—into storage requirements?
27. How do you differentiate between online searchable retention and long-term cold/archival retention requirements?
28. How do you determine log retention durations based on legal mandates, regulatory compliance (PCI DSS, ISO 27001, HIPAA, SOC 2), external audits, and internal security policies?
29. Beyond retention duration, what requirements must you establish for log immutability, access governance, cryptographic verification, archival storage, and compliant purging?
30. If storage budgets are constrained while regulatory retention mandates are strict, how do you prioritize requirements and architect tiered retention?

### Topic 07: Infrastructure & Network Requirements

31. How do you gather compute, virtualization, storage IOPS, network throughput, and operating system prerequisites for the SIEM deployment?
32. How do you identify the necessary network routes, firewall rules, and communication ports required to transmit logs across various VLANs and subnets to the SIEM?
33. What criteria do you use to evaluate whether the SIEM should be deployed on-premises, in the public cloud, or within a hybrid architecture?
34. How do you establish log collection requirements for remote endpoints and branch offices when WAN bandwidth is constrained?
35. Prior to deployment, how do you validate core infrastructure dependencies such as DNS resolution, NTP time synchronization, PKI/certificates, and corporate proxy configurations?

### Topic 08: Availability, Backup & Recovery Requirements

36. How do you determine the organization's acceptable downtime and availability SLA for the SIEM platform?
37. How do you define Recovery Time Objective (RTO) and Recovery Point Objective (RPO) for SIEM operational failures based on business impact?
38. How do you elicit distinct availability tiers for log collection, real-time stream processing, search indexing, and dashboard access?
39. What requirements must you establish for disaster recovery (DR), offsite backup locations, automated snapshotting, and recovery validation drills?
40. What requirements must you define for local agent spooling, collector queueing, and backlog drain rates during SIEM outages?

### Topic 09: Security, Access & Integration Requirements

41. What role-based access control (RBAC) tiers and privilege levels must you define for SIEM administrators, detection engineers, SOC analysts, and compliance auditors?
42. How do you capture requirements for integrating the SIEM with Active Directory (LDAP/SSO), IT ticketing platforms, alerting channels, and broader security ecosystems?
43. What authentication, authorization, secret storage, and network access requirements must be defined for SIEM API integrations?
44. How do you establish data protection requirements—including TLS in transit, encryption at rest, field-level masking, and PII anonymization—for sensitive log data?
45. What external dependencies, third-party integrations, and upstream/downstream security prerequisites must be documented prior to SIEM implementation?

### Topic 10: Project Scope, Constraints & Acceptance

46. How do you factor corporate budget, deployment milestones, engineering bandwidth, and licensing models into the SIEM architecture and implementation scope?
47. How do you select target endpoints and log sources for a pilot (Proof of Concept / Pilot Phase) deployment, and what acceptance criteria determine its success?
48. How do you document technical assumptions, architectural risks, prerequisite dependencies, and open technical questions during requirement assessment?
49. If organizational scope changes or new logging requirements emerge during the project, how do you handle scope reassessment, capacity replanning, and change control?
50. Upon completing the requirement assessment phase, what documentation, asset inventories, EPS baselines, and architectural inputs must be signed off before advancing to architecture design and infrastructure sizing?

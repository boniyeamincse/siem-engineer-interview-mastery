# 07 — Endpoint Agent Deployment

## SIEM Engineer Interview Questions

### Topic 01: Agent Architecture, Capabilities & OS Support

1. How does the internal architecture of an enterprise SIEM agent (e.g., Wazuh Agent, Elastic Agent, Splunk Forwarder) function, and does it operate primarily within user space or kernel space?
2. How do you manage agent packaging variants and software dependencies across heterogeneous operating systems (Windows 10/11, Windows Server 2016–2022, RHEL, Ubuntu, Rocky Linux)?
3. Beyond log collection, how do specialized agent sub-modules—such as File Integrity Monitoring (FIM), Rootkit Detection, Vulnerability Scanning, and Security Configuration Assessment (SCA)—execute on the endpoint?
4. How do you enforce service isolation and process priority on agents to ensure critical production host applications are never impacted by agent operations?
5. What architectural differences exist when deploying agents across Cloud Virtual Machines, Docker containers, and Kubernetes worker nodes compared to traditional bare-metal servers?

### Topic 02: Automated Agent Deployment & Orchestration

6. How do you orchestrate silent, non-disruptive agent installations across 500 Windows endpoints using Active Directory Group Policy Objects (GPO) or Microsoft Intune/SCCM?
7. How do you execute single-command agent deployment, configuration distribution, and service startup across a fleet of 50+ Linux servers using Ansible Playbooks?
8. When golden OS images or VM templates (VMware Templates / AMIs) contain pre-installed SIEM agents, how do you prevent and resolve duplicate Agent ID collisions upon cloning?
9. How do you design an agent distribution and auto-enrollment pipeline for remote branch laptops and teleworkers who lack continuous Active Directory connectivity?
10. How do you configure automated retry logic, error handling, and deployment failure alerting within deployment automation scripts?

### Topic 03: Agent Enrollment, Registration & Key Management

11. What is the technical handshake, cryptographic verification, and authentication workflow (`agent-auth`, SSL/TLS certificates, shared secrets) between an agent and the SIEM manager during registration?
12. How do you configure secure mass auto-enrollment across hundreds of endpoints using shared registration passwords or cryptographic enrollment tokens?
13. What is the role of the agent's unique symmetric encryption key (stored in `client.keys`), and why is a unique key generated for each enrolled host?
14. For dynamic DHCP endpoints and roaming laptops where IP addresses change frequently, how do you configure registration policies to prevent communication breakage?
15. If an endpoint agent's encryption key or credentials are compromised, what is the procedure to immediately revoke and blacklist that specific agent on the central manager?

### Topic 04: Centralized Agent Configuration & Group Management

16. How do you implement centralized agent group configuration via `agent.conf` on the SIEM manager, avoiding individual configuration edits on endpoints?
17. How do you organize endpoints into distinct functional groups (e.g., `Windows-Workstations`, `Linux-WebServers`, `Domain-Controllers`, `DB-Servers`) to push role-specific log collection policies?
18. When an endpoint belongs to multiple configuration groups simultaneously (multi-group assignment), how are configuration parameter conflicts and precedence resolved?
19. How does an agent dynamically poll and apply updated centralized configurations without requiring host reboots or disrupting end-users?
20. How do you ensure that an agent automatically validates XML/JSON syntax and rolls back to its last known good local configuration if a corrupted centralized policy is pushed?

### Topic 05: Resource Consumption & Host Protection

21. How do you configure resource throttling to enforce strict limits ensuring the agent never exceeds 1–2% CPU utilization and 100–150 MB RAM on the host machine?
22. During sudden endpoint log storms (e.g., an application generating millions of events), how do you configure agent-level event rate-limiting (`events_per_second` at agent level) to prevent host freezing?
23. What configurations do you apply to throttle agent network bandwidth usage and prevent saturation of branch WAN links?
24. How do you utilize `nice` and `ionice` (I/O scheduling classes: idle / best-effort) in Linux to prevent agent disk reads from impacting production database I/O?
25. How do you configure host-level self-healing (systemd watchdogs or service recovery policies) to handle agent crash loops or memory leaks safely?

### Topic 06: Local Disk Buffering & Network Disconnection Resiliency

26. How does the agent utilize its local disk buffer (spooler / persistent disk queue) to retain security events during network disconnections or internet outages?
27. What mathematical formula determines optimal local disk buffer sizing (e.g., 500 MB to 1 GB) to ensure events are preserved without exhausting client disk capacity?
28. When the local buffer saturates during extended network outages, what should the default behavior be—dropping older events (FIFO / tail-drop) or halting collection—and which is justifiable from a security perspective?
29. Upon network restoration, how do you regulate backlog drain rates so that hundreds of recovering agents do not overwhelm the central SIEM ingestion pipeline?
30. What mechanisms prevent data corruption within the agent's local disk spool files during abrupt host power loss or unexpected reboots?

### Topic 07: Remote Agent Upgrades & Lifecycle Management

31. How do you orchestrate batch-wise remote agent upgrades across hundreds of distributed endpoints from the central manager without requiring physical or manual host access?
32. If a network interruption occurs or package verification fails during an upgrade, how does the agent automatically roll back to its previous stable version to maintain monitoring continuity?
33. How do you conduct canary deployments for agent updates—advancing from a test lab, to a 5% canary cohort, and finally phased across the entire enterprise?
34. How do you maintain backward compatibility between updated SIEM managers and legacy operating systems (such as Windows Server 2008 or CentOS 6) that cannot run modern agent binaries?
35. When decommissioning or un-onboarding an endpoint, what automated workflows ensure complete removal of agent binaries, configuration files, registry keys, and local logs?

### Topic 08: Agent Connectivity Troubleshooting & Communication Edge Cases

36. When an agent is installed and its service is running but the SIEM manager reports it as "Disconnected" or "Never Connected," what step-by-step methodology do you use to diagnose the root cause?
37. How do you diagnose whether port 1514 (TCP/UDP) is blocked or dropped across intervening firewalls or NAT gateways using `nc`, `tcpdump`, or `Test-NetConnection`?
38. How do you identify and resolve silent connection drops or packet fragmentation caused by MTU size mismatches when agents transmit large event payloads?
39. How do you prevent mTLS encryption traffic corruption or blocking in environments enforcing enterprise proxy inspection or SSL/TLS decryption firewalls?
40. How do you resolve TLS handshake failures and agent certificate rejections caused by endpoint system clock drift?

### Topic 09: Asset Inventory Reconciliation & Coverage Auditing

41. What scripting methodologies or automated reconciliation workflows do you implement to cross-reference Active Directory/CMDB computer lists against active SIEM agents?
42. How do you leverage network sweeps, DHCP lease logs, and ARP telemetry to discover rogue or shadow IT devices operating on the network without a SIEM agent?
43. How do you automate cleanup policies on the central manager to purge stale or inactive agent keys from long-decommissioned or permanently offline endpoints?
44. What automated mechanisms do you build to generate a daily "Missing Agent Report" to maintain and verify 99%+ enterprise telemetry coverage?
45. When an endpoint is multi-homed or connects across multiple network interfaces (Ethernet, Wi-Fi, VPN), how do you prevent duplicate agent registrations and concurrent session conflicts?

### Topic 10: Anti-Tampering, Agent Hardening & Privileged Defense

46. What defense-in-depth strategies prevent local administrators or malicious actors from stopping (`net stop`, `systemctl stop`) or killing SIEM agent processes?
47. How do you configure filesystem ACLs, restricted directory permissions, and tamper protection to prevent unauthorized modification of agent binaries and configuration files?
48. What alerting thresholds and manager-side rules do you establish to instantly trigger high-severity alerts upon sudden agent disconnections or tampering attempts?
49. How do you configure code signing certificates and antivirus/EDR exclusion policies to ensure endpoint security solutions do not mistakenly quarantine SIEM agent binaries?
50. As a Lead SIEM Engineer, what compliance metrics, coverage audits, and operational test reports would you present to achieve 100% formal sign-off for a 500-endpoint rollout?

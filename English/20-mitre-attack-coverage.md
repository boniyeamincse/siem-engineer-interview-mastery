# 20 — MITRE ATT&CK Coverage

## SIEM Engineer Interview Questions

### Topic 01: MITRE ATT&CK Matrix Architecture & Taxonomy

1. What is the MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) framework, and why is it recognized as the universal common language for modern enterprise cyber defense?
2. How do Tactics (the adversary's tactical objectives—e.g., Initial Access, Privilege Escalation, Exfiltration) structurally relate to Techniques and Sub-techniques (the specific technical mechanisms used to achieve those objectives)?
3. What are the core architectural and contextual differences among the Enterprise Matrix, Cloud Matrix, and ICS/OT Matrix?
4. Procedure Level Telemetry: Why is understanding high-level technique definitions insufficient for detection engineers, and why must detection logic target specific adversarial procedures (concrete commands, tools, and scripts)?
5. How do you compare the Lockheed Martin Cyber Kill Chain with the MITRE ATT&CK framework, and why does ATT&CK provide superior granularity and behavioral depth for detection engineering?

### Topic 02: Mapping Detection Rules to ATT&CK Techniques

6. What is the standard methodology for accurately tagging a production detection rule with MITRE ATT&CK Technique and Sub-technique IDs (e.g., `T1059.001 - PowerShell`)?
7. Superficial Mapping Trap: How do you identify and prevent superficial mapping (where a rule checks trivial static strings but is misleadingly tagged with an advanced broad technique), avoiding a false sense of security?
8. When engineering a complex correlation rule that spans multiple attack phases (e.g., T1078 Valid Accounts -> T1059 Command Execution -> T1041 Exfiltration), how do you structure multi-technique mapping?
9. How do you define structured rule metadata fields for MITRE Tactic, Technique ID, Sub-technique, and authoritative reference URLs across diverse SIEM rule definitions?
10. How do you author and enforce "Detection Mapping Review Guidelines" to maintain consistency, accuracy, and engineering rigor across the enterprise rule repository?

### Topic 03: MITRE ATT&CK Navigator & Heatmap Engineering

11. What is the MITRE ATT&CK Navigator tool, and how do you leverage it to generate comprehensive enterprise detection coverage heatmaps using Navigator Layer JSON schemas?
12. What scoring logic and gradient color coding schemes (e.g., dark green = robust multi-procedure detection, yellow = partial coverage, red = zero visibility) do you configure in coverage heatmaps?
13. How do you design an automated Python script that parses the detection repository's rule files and dynamically compiles an up-to-date Navigator Layer JSON file?
14. How do you architect multi-layer composite views in Navigator (e.g., Layer 1 = Endpoint/EDR, Layer 2 = Network/NTA, Layer 3 = Cloud telemetry) to overlay and assess holistic defensive visibility?
15. What architectural pipeline do you use to embed and render live, interactive ATT&CK Navigator heatmaps within SOC operational dashboards or intranet security portals?

### Topic 04: Detection Gap Analysis & Blind Spot Discovery

16. Detection Gap Analysis: What systematic analytical process do you follow to identify enterprise tactics and techniques that lack visibility or detection coverage?
17. Visibility Gap (requisite telemetry not ingested) versus Detection Gap (logs ingested but lacking correlation rules): What are their operational differences, and how do you prioritize remediation?
18. If an enterprise gap assessment exposes near-zero coverage across Credential Access (TA0006) and Lateral Movement (TA0008), how do you evaluate the immediate breach risk and prioritize remediation?
19. Does a high gross rule count equate to robust security coverage? (e.g., having 50 rules for T1059 but zero coverage across 100 other enterprise techniques—how do you remediate this imbalance?)
20. How do you translate detection gap analysis findings into an actionable, prioritized quarterly "Detection Engineering Roadmap & Engineering Backlog"?

### Topic 05: Data Source Dependency Mapping (Data Components)

21. How do MITRE ATT&CK Data Sources and Data Components (e.g., `Process: Process Creation`, `Network Traffic: Network Connection`, `File: File Modification`) directly correlate with detection engineering prerequisites?
22. Prior to authoring a detection rule for a specific technique (e.g., T1003.001 LSASS Memory Dump), what audit steps verify that the underlying Data Component (`Process: Process Access`) is actively enabled and ingested?
23. Data Source Coverage Matrix: How do you perform a mathematical cross-impact analysis to identify which single log source integration will unlock visibility across dozens of unmonitored ATT&CK techniques?
24. What specific behavioral blind spots are created when endpoint auditing tools like Sysmon or Linux auditd are absent, and how do you document these data source dependencies?
25. Explain how degraded data quality, malformed schemas, or missing normalized fields can silently invalidate ATT&CK detection mapping in production.

### Topic 06: Threat-Informed Defense & Threat Actor Profiling

26. Threat-Informed Defense: How do you prioritize detection engineering coverage against Advanced Persistent Threat (APT) groups (e.g., APT29, FIN7, Lazarus Group) known to target your specific industry vertical (e.g., Financial Services, Healthcare)?
27. What is the process for extracting threat actor TTP profiles from MITRE ATT&CK intelligence feeds and cross-referencing them against your enterprise detection repository to identify coverage deficits?
28. Ransomware Execution Playbooks: How do you map and validate complete kill-chain technique coverage spanning Initial Access, Defense Evasion, Lateral Movement, and Data Encrypted for Impact?
29. Threat Delta Analysis: When the Cyber Threat Intelligence (CTI) team publishes an emergency threat bulletin, how do you rapidly ingest the associated TTPs into Navigator to evaluate immediate exposure?
30. Chokepoint Detection Strategy: What is the architectural philosophy behind ensuring robust, high-fidelity detection at critical operational bottlenecks across an adversary's kill chain?

### Topic 07: Coverage Scoring Models & Maturity Metrics

31. Why is Binary Coverage (assuming 100% protection simply because a single rule maps to a technique name) dangerous, and how does it introduce catastrophic organizational complacency?
32. Depth of Coverage: When a complex technique (e.g., T1059.001) encompasses over 30 distinct execution procedures, what methodology do you use to score and quantify granular procedural coverage?
33. What is the MITRE D3FEND framework, and how does it complement ATT&CK by providing a standardized matrix for designing defensive countermeasures and detection components?
34. How do you establish an enterprise Detection Maturity Model (e.g., Level 0: No logs; Level 1: Logs ingested; Level 2: Baseline rule active; Level 3: Validated & tuned; Level 4: Automated SOAR response)?
35. How do you utilize open-source frameworks such as `DeTT&CT` (Detect Tactics, Techniques & Combat Threats) to mathematically score and visualize defensive readiness?

### Topic 08: Validation via Adversary Simulation & Evidence

36. What scientific methodology transitions detection coverage validation from theoretical paper-based compliance to empirical adversary simulation?
37. How do you enforce a governance rule where a technique is only certified as "Verified / Tested Coverage" (green on heatmaps) if it successfully passes an automated Atomic Red Team execution?
38. When an adversary simulation test fails (the simulated attack executes successfully but zero alerts are generated), what automated workflow downvotes coverage status on operational dashboards?
39. Purple Teaming Exercises: How do you structure and execute collaborative Purple Team engagements to systematically test, validate, and iteratively engineer detections across the ATT&CK matrix?
40. How do you design and maintain an "ATT&CK Validation Audit Repository" that permanently links test execution logs, raw telemetry, and alert JSON evidence to specific technique IDs?

### Topic 09: Matrix Updates, Deprecations & Maintenance

41. MITRE routinely publishes updates to the ATT&CK framework (e.g., upgrading from v13 to v14 or v15)—how do you maintain continuous synchronization when techniques are restructured or deprecated?
42. What programmatic procedures and scripts do you use to scan rule repositories, detect deprecated technique IDs, and automatically remap them to updated Sub-technique taxonomies?
43. Following an upstream ATT&CK schema update, how do you run automated regression tests to verify that internal tagging schemas and reporting integrations remain intact?
44. What change management process governs the rapid triage and integration of newly introduced Cloud, Container, or Identity techniques into the enterprise detection backlog?
45. How do you track, quantify, and report multi-year coverage progression metrics (e.g., maturing from 30% validated coverage in 2023 to 75% in 2024) to leadership?

### Topic 10: Executive Reporting, Posture & Business Justification

46. How do you translate technical MITRE ATT&CK coverage metrics into high-level, business-aligned executive dashboards and risk posture reports for the CISO and Board of Directors?
47. Business Case Justification: How do you structure a CAPEX/OPEX proposal demonstrating: "Without deploying Sysmon or activating this audit log source across Domain Controllers, we remain blind to 10 critical Tier-1 adversary techniques"?
48. How do you package verified MITRE ATT&CK coverage reports as authoritative compliance artifacts for Cyber Insurance underwriting, ISO 27001 audits, and regulatory examinations?
49. What standardized "ATT&CK Coverage RFP Checklist" do you establish to objectively evaluate and benchmark the detection capabilities of third-party MSSPs and MDR providers?
50. As an Enterprise Principal Detection Architect, how do you author, govern, and obtain formal executive sign-off on a comprehensive "MITRE ATT&CK Coverage & Engineering Maturity Charter"?

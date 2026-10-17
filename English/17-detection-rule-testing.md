# 17 — Detection Rule Testing

## SIEM Engineer Interview Questions

### Topic 01: Detection Testing Methodology & Test Frameworks

1. Why is it mandatory to conduct comprehensive Positive Testing, Negative Testing, and Regression Testing prior to deploying a detection rule into production?
2. What is the fundamental operational difference between validating a rule query against historical static test data versus executing an end-to-end detection pipeline test (Event Generation -> Log Ingestion/Parsing -> Rule Evaluation -> Alert Firing -> SOC Notification)?
3. How do you architect an isolated Staging/Lab environment for detection testing, and how do you ensure environmental and configuration parity with the production SIEM deployment?
4. What are the three golden pass criteria (True Positive Alert Generation, Correct Severity/Metadata Assignment, and Zero Undesired Side Effects) required to certify a detection test as successful?
5. If SecOps or management requests fast-tracking an untested rule directly into production, what critical technical and operational risks (e.g., alert storms, indexer performance degradation, cascading false positives, system lockouts) do you articulate to mandate testing?

### Topic 02: Positive Testing & True-Positive Verification

6. What is the primary objective of positive testing, and how do you simulate a specific adversary scenario (e.g., LSASS memory dumping) to empirically prove that a detection rule triggers as expected?
7. Following rule triggering with synthetic or simulated attack telemetry, what verification process do you execute to validate that every alert schema field (e.g., `threat.technique.id`, `user.name`, `source.ip`, `process.parent.name`) is accurately populated?
8. If a positive test fails to generate an alert, what systematic troubleshooting methodology do you use to isolate the root cause among detection logic flaws, telemetry ingestion gaps, or parser decoding mismatches?
9. How does test case design differ when developing positive tests across diverse operating systems and architectures, such as Windows endpoints, Linux servers, and cloud control planes?
10. How do you incorporate canary identifiers and metadata tags (e.g., `test_run_id: "QA-SEC-101"`) into simulated attack traffic to prevent test alerts from generating noisy false incident tickets in the production SOC ticketing system?

### Topic 03: Negative Testing & False-Positive Proofing

11. What is negative testing in detection engineering, and how do you execute tests against legitimate administrative activities and baseline user traffic to validate false-positive resistance?
12. Example: When conducting negative testing for a `powershell.exe -enc` detection rule, how do you execute legitimate IT automation tasks (e.g., SCCM client health check scripts or Ansible modules) to verify the rule does not fire?
13. How do you stress-test a detection rule against high-volume benign telemetry generated during standard enterprise operations, such as routine software installations or application updates?
14. If negative testing reveals that benign administrative traffic triggers the detection rule, what structured process do you follow to refine exclusions, whitelisting parameters, or exception logic without creating detection blind spots?
15. How do you balance negative testing to avoid over-fitting (making a detection rule overly restrictive to the point where minor attack variations evade detection)?

### Topic 04: Adversary Simulation Tooling (Atomic Red Team, Caldera)

16. How do you utilize the open-source adversary simulation framework Atomic Red Team (by Red Canary) to execute automated positive test cases mapped to specific MITRE ATT&CK techniques?
17. What is the standard operational procedure for conducting multi-stage, end-to-end attack simulation tests using frameworks like MITRE Caldera or Infection Monkey?
18. Provide concrete syntax and operational commands to invoke an atomic test for a targeted technique (e.g., T1059.001 PowerShell, T1003.001 LSASS Memory, or T1078 Valid Accounts) and subsequently execute automated artifact cleanup.
19. What critical safety guardrails (e.g., disabling destructive payloads, utilizing non-production service accounts, restricting network scope) must be verified before executing adversary simulation tests in production or production-adjacent systems?
20. How do you correlate adversary simulation execution timestamps with SIEM alert generation timestamps to calculate and baseline Time-to-Detect (TTD) metrics?

### Topic 05: Regression Testing & Detection Integrity

21. When a log parser/decoder is modified or the SIEM platform undergoes a major version upgrade, what regression testing strategy ensures that hundreds of existing production rules remain fully functional?
22. What is a "Detection Regression Suite", and how do you design an automated batch testing framework that replays curated repositories of historical raw logs following codebase modifications?
23. How do you prevent and detect hidden side effects—such as a single regex modification in a shared decoder breaking field extraction across ten unrelated detection rules—using automated regression tests?
24. What is the recommended execution frequency for running full-suite regression tests across an enterprise detection repository, and how do you optimize testing automation pipelines?
25. How do you configure CI/CD pipeline webhooks and automated gates to block Git commits or Pull Requests when a regression test fails, notifying the responsible detection engineer?

### Topic 06: Controlled Attack Simulation in Staging vs Production

26. What are the common underlying causes when an attack simulation triggers 100% reliably in a staging lab but completely fails in production (e.g., firewall filtering policies, disparate audit logging policies, varying EDR enforcement modes)?
27. What standard operating procedures (SOP), formal Change Management windows, and stakeholder alignment (SecOps, IT, Infrastructure) are required to conduct controlled adversary simulation in live production environments?
28. What are the operational trade-offs and methodologies between coordinated testing (notifying on-duty SOC analysts in advance) and unannounced blind testing to evaluate real-world SOC detection and incident response capabilities?
29. How do you design and maintain an emergency "Kill Switch" and rollback procedure to instantly terminate adversary simulation scripts if a test causes thread hangs, high CPU spikes, or port exhaustion on production hosts?
30. What post-test validation procedures do you perform to ensure all test files, temporary testing credentials, simulated registry modifications, and transient artifacts are comprehensively purged?

### Topic 07: Testing Time-Window & State-Machine Correlations

31. How do you design precisely timed, multi-stage test cases to validate stateful correlation rules (e.g., "3 failed logon attempts followed by 1 successful logon within 5 minutes")?
32. Time-Window Boundary Testing: How do you validate edge conditions by injecting a successful authentication event at exactly 4 minutes 59 seconds (confirming rule triggers) versus 5 minutes 01 second (confirming state expiration and rule drop)?
33. How do you simulate out-of-order log delivery (e.g., where Event 2 arrives before Event 1 due to transport latency or agent buffering) to ensure the correlation state machine accurately reconstructs event order?
34. How do you script cross-host correlation testing involving distributed telemetry (e.g., a port scan initiated from Host A triggering simultaneous process creation on Host B)?
35. When testing long-duration correlation rules (e.g., low-and-slow data exfiltration spanning a rolling 24-hour window), what test acceleration techniques or synthetic timestamp manipulation methods do you use to avoid waiting 24 real-time hours?

### Topic 08: Obfuscation & Defense Evasion Testing

36. How do you test whether command-line detection rules can successfully withstand adversary evasion techniques, such as PowerShell backticks (`` ` ``), cmd caret escaping (`^`), environment variable concatenation, and Base64 encoding?
37. Provide a testing methodology for validating a detection rule against ten diverse obfuscated representations of a command (e.g., `powershell.exe -w hidden -nop -enc ...`).
38. How do you execute binary renaming and masquerading tests (e.g., renaming `mimikatz.exe` to `svchost.exe` or `test.exe`) to demonstrate that a rule relies on behavioral heuristics, PE metadata, or hash lookups rather than brittle file names?
39. What techniques do you use to test detection resilience against command-line space padding (inserting hundreds of random spaces to bypass string matching) and DOS 8.3 short file names (`PROGRA~1`)?
40. Following evasion testing, how do you harden detection logic by implementing de-obfuscation functions, normalized whitespace parsing, and case-insensitive matching?

### Topic 09: Test Automation & CI/CD Integration

41. In a Detection-as-Code engineering pipeline, how do you configure automated continuous testing triggered by Git pull requests using platforms such as GitHub Actions or GitLab CI?
42. How do you architect unit testing for detection rules using Python frameworks like `pytest`, feeding raw sample log fixtures into the test runner and asserting expected alert outputs?
43. How do you configure automated CI/CD branch protection rules to block PR merging and send build failure logs to engineers whenever detection tests fail?
44. What is the target execution runtime for running hundreds of automated test cases in a staging pipeline, and how do you parallelize test execution to optimize CI/CD feedback cycles?
45. How do you containerize the entire detection testing suite (Dockerized Test Runner) so detection engineers can execute standardized tests locally on their workstations before committing code?

### Topic 10: Detection QA Sign-Off & Verification Evidence

46. What formal Quality Assurance (QA) sign-off process must a detection rule satisfy before being approved for production deployment in an enterprise environment?
47. What essential components must be assembled into a Detection Test Evidence Package (e.g., positive test logs, raw alert JSON payloads, negative test validation logs, and query runtime execution metrics)?
48. Test Reproducibility: How do you document test cases and test automation scripts so that any peer engineer can execute identical test procedures in the future and achieve deterministic results?
49. How do you calculate, define, and present a quantifiable "Detection Readiness Score" to SecOps managers and executive leadership prior to rule commissioning?
50. What governance checklist governs the formal transition of a detection rule from "Development/Staging Backlog" to "Tested & Production-Approved" status?

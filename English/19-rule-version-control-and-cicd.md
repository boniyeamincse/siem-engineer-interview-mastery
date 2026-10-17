# 19 — Rule Version Control and CI/CD

## SIEM Engineer Interview Questions

### Topic 01: Detection-as-Code (DaC) Principles & Git Repository Architecture

1. What is Detection-as-Code (DaC), and why has managing detection rules and decoders in a Git repository like software engineering source code superseded manual web console configuration as the modern enterprise standard?
2. How do you architect the directory structure of an enterprise DaC repository (e.g., `/rules`, `/decoders`, `/tests`, `/mappings`, `/ci-cd`, `/docs`)?
3. How do you structure a multi-format repository to concurrently support heterogeneous platforms (e.g., Wazuh XML, Sigma YAML, Splunk `savedsearches.conf`, Elastic EQL)?
4. Rule Metadata Tracking: How do you enforce structured comments or YAML headers to track rule author, unique UUID, semantic version number, creation timestamp, and changelog history?
5. How does implementing Detection-as-Code accelerate the SOC's Detection Development Lifecycle (DDLC) and reduce Mean Time to Detect (MTTD)?

### Topic 02: Branching Strategies & Peer Review Workflows

6. Which Git branching strategy (e.g., GitFlow vs. GitHub Flow / Trunk-Based Development) is optimal for a collaborative Detection Engineering team, and why?
7. How do you enforce branch protection rules to strictly prevent direct commits or pushes to production branches (`main` or `production`), mandating peer-reviewed Pull Requests (PRs)?
8. How do you implement a `CODEOWNERS` policy requiring mandatory sign-off from at least two senior peer reviewers before any detection rule or decoder PR can be merged?
9. Peer Review Checklist: What critical technical criteria (e.g., logic flaws, query performance impact, catastrophic ReDoS backtracking, false positive exposure) must a reviewer evaluate during a detection PR review?
10. Emergency Hotfixes (Zero-Day Rule Deployments): How do you architect an expedited emergency deployment bypass window and post-incident compliance audit trail for rapid rule publishing during an active cyber incident?

### Topic 03: Automated Linting & Static Code Analysis

11. How do you configure automated linting in the CI/CD pipeline (e.g., XML schema validation, YAML linting, JSON schema enforcement) triggered upon code push?
12. Static Code Analysis: How do you author custom linters to scan detection queries for risky, unoptimized syntax patterns (e.g., leading wildcards `*query*`, unbounded search timeframes, missing index tags) and fail the pipeline build?
13. Secret Scanning: How do you integrate automated secret scanning tools like `gitleaks` or `trufflehog` into pre-commit hooks and CI pipelines to block hardcoded API tokens, passwords, or credentials?
14. Enforcing Naming Conventions & Style Guides: How do you write validation scripts to verify rule ID ranges (e.g., custom IDs 100000–199999), severity levels, and description formatting against organizational standards?
15. How do you configure CI runners (GitHub Actions / GitLab Runner) to abort the pipeline and dispatch structured notifications to engineers when linting checks fail?

### Topic 04: Automated Unit & Integration Testing in CI

16. How do you execute automated unit tests in the CI pipeline by supplying mock raw event logs and asserting expected alert outputs for each detection rule?
17. How do you spin up lightweight, containerized SIEM engines (e.g., headless Wazuh manager or Elastic test containers) inside Dockerized CI runners to test real decoding and rule evaluation logic?
18. Negative Test Automation: How do you execute test suites of benign enterprise log telemetry in the CI pipeline to verify that newly authored rules generate zero unexpected false positives?
19. Regression Test Suites: When modifying a core decoder regex, how do you parallelize the execution of thousands of regression test cases to achieve deterministic verification within 2 to 3 minutes?
20. Test Coverage Metrics: How do you track and display the percentage of production detection rules backed by active, automated test fixtures on a central engineering dashboard?

### Topic 05: Continuous Delivery & Deployment to Staging/Production

21. Following successful automated test runs, how does the CD pipeline automatically package and deploy validated rule and decoder artifacts to Staging, and subsequently promote them to Production?
22. How do you implement push-based zero-touch deployment using Ansible Playbooks, Terraform, or SIEM REST APIs without requiring manual SSH logins or manual file transfers?
23. Canary Rule Deployments in CI/CD: How do you automate the progressive delivery of new rules—first deploying to a staging cluster, then to a 5% production canary node cohort in silent mode, before widespread release?
24. In distributed multi-manager or clustered SIEM architectures, how do you guarantee atomic configuration synchronization across all worker nodes during automated deployments?
25. Manual Approval Gates: How do you configure pipeline promotion gates requiring formal click-to-approve sign-off from SecOps leadership between staging validation and production rollout?

### Topic 06: Zero-Downtime Rule Reload & API Synchronization

26. How do you trigger zero-downtime hot reloading of updated rules and decoders via SIEM APIs or signals without restarting the underlying SIEM ingestion services?
27. Why do full service restarts risk dropped packets or ingestion pipeline buffering lag, and how does live hot-reloading eliminate ingestion disruption?
28. How do you run automated pre-flight syntax checks (e.g., `wazuh-analysisd -t` or syntax verification APIs) in the pipeline to confirm zero configuration errors before issuing the reload command?
29. If an individual worker node fails to reload its configuration, what clustering safeguards prevent configuration drift and split-brain desynchronization across the SIEM cluster?
30. How do you monitor and baseline memory footprints and CPU utilization spikes across SIEM nodes immediately following a large-scale rule reload?

### Topic 07: Automated Rollback & Fault Recovery

31. If a production rule reload causes process crashes or 100% memory consumption on worker nodes, how does the CI/CD pipeline detect health check failures and trigger an automated immediate rollback?
32. How do you execute an automated `git revert` to the last known stable commit and restore the production cluster to a verified Last Known Good State within 60 seconds?
33. Following an automated rollback, what telemetry, crash dumps, and pipeline execution logs are captured and preserved to support post-mortem and Root Cause Analysis (RCA)?
34. Partial Failure Handling: How do you remediate transient partial failures where a rule deployment succeeds on two cluster nodes but drops on a third due to network timeouts?
35. What automated notification channels are dispatched to alert SOC operations and detection engineers when an emergency rollback sequence is triggered and successfully executed?

### Topic 08: Out-of-Band Change Prevention & Single Source of Truth

36. Out-of-Band Configuration Drift: How do you prevent conflicts between Git and production caused by administrators or analysts modifying rules directly in the SIEM web console or via ad-hoc SSH?
37. How do you lock production rule directories as read-only (e.g., using `chattr +i` or restrictive OS file permissions) to allow write access exclusively to the CI/CD service identity?
38. Configuration Drift Detection: How do you schedule continuous drift detection jobs that calculate cryptographic hashes of production rule directories and compare them against the Git HEAD commit?
39. "Git is the Single Source of Truth": When unauthorized manual modifications are detected in production, how do you implement auto-healing routines that automatically overwrite production with the Git state?
40. How do you disable or hide interactive rule-editing interfaces in the SIEM web console to enforce Git as the sole configuration mechanism?

### Topic 09: Secrets Management & CI/CD Security Hardening

41. How do you eliminate hardcoded plaintext credentials from CI/CD pipelines by integrating enterprise secrets management solutions like HashiCorp Vault, AWS Secrets Manager, or GitHub Encrypted Secrets?
42. How do you enforce the Principle of Least Privilege on service account tokens used by the deployment pipeline, restricting API permissions strictly to rule management endpoints?
43. What security advantages are gained by deploying self-hosted, ephemeral CI/CD runners inside a dedicated, isolated enterprise management network rather than using public cloud runners?
44. Supply Chain Risk Mitigation: How do you safeguard CI/CD pipeline dependencies and third-party plugins using lock files (`package-lock.json`, `poetry.lock`) and cryptographic checksum verification?
45. What automated procedures and rotation schedules do you implement to rotate deployment API tokens, SSH private keys, and service credentials every 90 days?

### Topic 10: CI/CD Governance, Audit Trails & Compliance Sign-Off

46. How do you generate an immutable, end-to-end digital audit trail (change request, peer approver, CI test results, deployment timestamp) to satisfy SOC 2 Type II, ISO 27001, and PCI-DSS compliance audits?
47. How do you write pre-commit or CI hooks that mandate linking every Git commit and Pull Request title to a verified Jira or ServiceNow Change Request (CR) ticket ID?
48. How do you automate the generation of structured, human-readable Release Notes summarizing rule additions, deprecations, and bug fixes to broadcast to the SOC team upon each release?
49. If an external auditor requests justification and approval documentation for a rule modified three months prior, what Git log and pull request metadata commands do you use to extract full evidence?
50. How do you compile and present a comprehensive "Detection-as-Code & CI/CD Pipeline Maturity Report" to the Head of Security Engineering demonstrating operational velocity and governance rigor?

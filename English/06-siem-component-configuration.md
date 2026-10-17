# 06 — SIEM Component Configuration

## SIEM Engineer Interview Questions

### Topic 01: Component Decoupling & Installation Sequence

1. When configuring Manager, Indexer, Dashboard, and Syslog Collector across decoupled, dedicated servers in a distributed enterprise SIEM, how do you determine the deployment sequence, and why?
2. What buffering issues, socket errors, or data loss risks occur if the SIEM Manager service is started before the Indexer cluster is fully initialized and in a "Green" health state?
3. When migrating configuration from a single-node all-in-one architecture to a multi-node distributed deployment, how do you reconfigure IP binding parameters (`bind_address`, `network.host`) for each component?
4. How do you configure and segment service ports across Linux OS firewalls and network ACLs (e.g., Manager: 1514/1515, Indexer: 9200/9300, Dashboard: 5601/443, API: 55000)?
5. How do you leverage systemd service dependencies (`After=`, `Requires=`, `Wants=`) to ensure automated, ordered component startup following server reboots or power outages?

### Topic 02: Mutual TLS (mTLS) & Certificate Authority Engineering

6. How do you generate and manage a dedicated internal Certificate Authority (Root CA and Intermediate CA) to secure all inter-component communication (Agent-to-Manager, Manager-to-Indexer, Dashboard-to-Indexer)?
7. Why is Mutual TLS (mTLS) necessary for node-to-node communication, and how do you enforce bidirectional certificate validation on both client and server endpoints?
8. Why is issuing distinct node certificates with unique Subject Alternative Names (SANs—FQDN and IP) significantly more secure than deploying wildcard certificates (`*.domain.local`)?
9. How do you configure automated certificate expiration tracking and zero-downtime certificate rotation to prevent cluster-wide pipeline outages due to expired certificates?
10. How do you secure private key filesystem permissions (`chmod 600`, `chown`) when generating certificates via open-source utilities (e.g., Wazuh certs tool, OpenSearch `securityadmin.sh`, Elastic `elasticsearch-certutil`)?

### Topic 03: SIEM Manager & Engine Architecture

11. How do you allocate roles between Master Nodes and Worker Nodes in a SIEM Manager cluster, and how is cluster-wide file synchronization (`cluster.json`, custom rules, decoders, CDB lists) managed across workers?
12. How do you configure Manager socket buffers (`remoted` queue size, `analysisd` queue size) to prevent event drops and buffer overflows during high-throughput traffic bursts?
13. How do you securely configure log levels, debugging thresholds, alerting limits, and external email/webhook notification channels in the Manager's global configuration (`ossec.conf`)?
14. How do you protect the connected agent state database and cryptographic keys (`client.keys`) from corruption or unauthorized access within the Manager backend?
15. In a multi-manager cluster, if the active Master node fails, how do Worker nodes continue processing using local caches, and how does the cluster prevent Split-Brain scenarios?

### Topic 04: SIEM Indexer & Cluster Node Engineering

16. In a 3-node indexer cluster, how do you define the roles of Master-Eligible Nodes, Data Nodes, and Ingest/Coordinating Nodes within the configuration file (`opensearch.yml` or `elasticsearch.yml`)?
17. How do you configure cluster discovery and voting settings (`cluster.initial_master_nodes`, `discovery.seed_hosts`) to guarantee quorum and permanently avoid split-brain conditions?
18. How do you tune shard distribution and write threadpools (`thread_pool.write.queue_size`) on data nodes to prevent `429 Too Many Requests (EsRejectedExecutionException)` errors during heavy ingest bursts?
19. What are the operational benefits and data durability risks of setting asynchronous translog flushes (`index.translog.durability: async`) and extending the index refresh interval (`index.refresh_interval: 30s`)?
20. What is the correct procedure and operational safeguard when using administrative CLI scripts (such as `securityadmin.sh`) to initialize security plugins and push credential updates?

### Topic 05: Index Lifecycle Management (ILM / ISM) & Templates

21. How do you design Index State Management (ISM) or Index Lifecycle Management (ILM) policies to automate index creation, rollover criteria, and tiered storage transitions?
22. How do you configure lifecycle transition triggers between Hot Phase (active ingest and search on NVMe), Warm Phase (read-only queries on SATA SSD), and Cold/Delete Phase (retention expiry, S3 archiving, or automated deletion)?
23. How do you construct a production Index Template to govern index settings and field mappings, preventing mapping explosions when unvetted dynamic fields enter the cluster?
24. Why is primary shard sizing generally targeted between 30 GB and 50 GB, and how do you configure auto-rollover policies (`max_primary_shard_size: 50gb`, `max_age: 30d`) to enforce this?
25. If automated index purging policies fail upon retention expiration—leading to disk saturation—how do you diagnose and resolve ISM/ILM policy execution errors?

### Topic 06: Dedicated Collector & Forwarder Configuration

26. When deploying a dedicated Syslog collector (e.g., Rsyslog, Logstash, Vector, or Fluent Bit), how do you structure buffering and processing pipelines for both TCP and UDP log ingestion?
27. How do you configure memory buffers and disk-assisted persistent queues in Rsyslog (`$ActionQueueType LinkedList`, `$ActionQueueFileName`) to buffer logs during downstream SIEM outages?
28. How do you author ingestion pipeline filter rules at the collector layer to discard or drop noisy, low-value logs from firewalls or proxies prior to forwarding?
29. How do you configure multi-threaded input listeners and output batch workers on collector hosts to ensure the collector itself never becomes an ingestion bottleneck?
30. How do you enable Mutual TLS (mTLS) encryption and wire compression when forwarding logs from collectors to central SIEM managers or indexers?

### Topic 07: Web UI & Dashboard Engine Configuration

31. How do you configure multi-node backend indexer URLs (`opensearch.hosts`) redundantly in the Dashboard configuration file (OpenSearch Dashboards / Kibana / Wazuh Dashboard)?
32. How do you enforce mandatory HTTPS redirection and integrate custom enterprise SSL/TLS certificates into the Dashboard web engine?
33. What are the operational and security benefits of placing an Nginx or HAProxy reverse proxy in front of the Dashboard, and how do you enforce HTTP security headers (HSTS, CSP, X-Frame-Options)?
34. How do you configure cookie session management, idle session timeouts, and browser caching policies on the SIEM dashboard in compliance with enterprise security policies?
35. When the Dashboard displays "Server Not Ready Yet" or "Cluster Red," how do you diagnose backend connectivity, credential authentication, and keystore configurations?

### Topic 08: API Connectivity, Authentication & Secrets

36. How do you secure SIEM Manager RESTful API endpoints by restricting IP bind addresses, altering default ports, and safeguarding administrative credentials?
37. How do you configure JSON Web Token (JWT) parameters, token expiration lifecycles, and refresh token policies for programmatic API authentication?
38. How do you avoid storing plaintext credentials in configuration files (`kibana.yml`, `opensearch.yml`) by leveraging secure credential keystores (`opensearch-keystore`, `kibana-keystore`)?
39. How do you provision dedicated service accounts with granular Role-Based Access Control (RBAC) to restrict third-party tools (SOAR, ticketing platforms) to least-privilege API endpoints?
40. In the event of an API service account or administrative credential compromise, what is the procedure to instantly invalidate all active JWT tokens and rotate API secrets?

### Topic 09: Component Health, Heartbeats & Diagnostics

41. How do you leverage cluster monitoring endpoints (`_cluster/health`, `_cat/indices`, `_cat/shards`, and `_nodes/stats`) to feed automated SIEM telemetry into external monitoring systems?
42. What technical cluster conditions cause cluster health to transition from "Green" to "Yellow," and from "Yellow" to "Red," and what immediate triage steps are required for each state?
43. How do you tune heartbeat and keep-alive intervals between agents and managers to eliminate false-positive "Agent Disconnected" alerts across unstable networks?
44. How do you configure log rotation (`logrotate`) for internal SIEM component logs (`/var/ossec/logs/ossec.log`, `/var/log/opensearch/cluster.log`) to prevent local disk exhaustion?
45. When shards enter an unassigned state within the cluster, how do you utilize the `_cluster/allocation/explain` API to diagnose root causes and safely force shard reallocation?

### Topic 10: Configuration Drift, Version Control & Production Verification

46. How do you implement Configuration-as-Code (CaC) using a centralized Git repository and Ansible to manage and audit SIEM configuration changes across all nodes?
47. How do you build an automated CI/CD staging pipeline that executes schema validation, XML syntax checks, and YAML linting prior to pushing configurations to production?
48. How do you execute graceful configuration reloads without triggering full service restarts to achieve zero-downtime production updates?
49. If a misconfigured setting causes a SIEM component to crash upon deployment, how does automated rollback revert the host to the last known good configuration?
50. What comprehensive "Production Readiness Verification Checklist" do you execute to formally validate ingestion, indexing, API responsiveness, and dashboard integrity prior to project sign-off?

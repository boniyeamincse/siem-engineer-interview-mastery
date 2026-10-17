# 12 — Log Parsing and Decoders

## SIEM Engineer Interview Questions

### Topic 01: Core Parsing Mechanics & Ingestion Engines

1. What are the foundational mechanics of log parsing within SIEM ingestion engines, and why is transforming raw unstructured text into structured key-value/JSON fields mandatory for efficient indexing and detection?
2. What are the comparative performance and CPU overhead differences among Regular Expressions (Regex), Grok patterns, Dissect plugins, and native JSON parsers?
3. Why does the `Dissect` plugin (which splits tokens purely based on delimiters and static offsets) perform orders of magnitude faster than complex regular expressions on high-throughput log streams?
4. How does a hierarchical parent-child decoder architecture function, and why is it an engineering best practice to match broad log families first before delegating to granular child decoders?
5. What are the architectural trade-offs between Schema-on-Write (parsing and indexing structured fields at ingestion time) and Schema-on-Read (parsing raw strings dynamically at query execution)?

### Topic 02: Regular Expression (Regex) Optimization & ReDoS Prevention

6. How does Regular Expression Denial of Service (ReDoS) / catastrophic backtracking occur in log parsing pipelines, and how can a poorly written regex consume 100% CPU and stall an entire SIEM cluster?
7. What is the operational distinction between greedy quantifiers (`.*`) and non-greedy/lazy quantifiers (`.*?`), and why is the unconstrained use of `.*` hazardous in production decoders?
8. How do atomic grouping (`(?>...)`) and possessive quantifiers (`.*+`) optimize parsing performance by suppressing unnecessary regex backtracking states?
9. Provide an example of optimizing an inefficient regex pattern (e.g., rewriting an IP address or email regex to eliminate backtracking while maximizing matching throughput).
10. What are the performance advantages of utilizing character classes (e.g., `[^ ]+` or `\S+`) instead of arbitrary wildcard matching when parsing space-delimited log strings?

### Topic 03: Custom Decoder Development & Wazuh Ruleset Integration

11. What is the syntax structure and schema required to author custom XML decoders (`/var/ossec/etc/decoders/local_decoder.xml`) for unsupported applications in Wazuh?
12. What is the architectural significance of the `<prematch>` tag in Wazuh decoders, and how does it prevent expensive regex operations from executing against irrelevant log streams?
13. How does field extraction mapping work between the `<regex>` and `<order>` tags in Wazuh decoders, and how do you bind normalized attributes like `srcip`, `dstip`, `srcuser`, `action`, and `status`?
14. When an application generates uniform headers but emits five divergent message body formats, how do you architect one parent decoder linked to five specialized child decoders?
15. How do you ensure custom decoders take precedence over default out-of-the-box system decoders in Wazuh without overwriting vendor core rulesets?

### Topic 04: Logstash, Vector & Fluentbit Pipeline Parsers

16. How do you construct an end-to-end, real-time log parsing pipeline in Logstash leveraging `grok`, `kv`, `mutate`, `json`, and `date` filter plugins in unison?
17. How do built-in Grok patterns (e.g., `%{IP:source_ip}`, `%{TIMESTAMP_ISO8601:timestamp}`, `%{WORD:method}`) operate under the hood in Logstash?
18. How does Vector's native Vector Remap Language (VRL) achieve high-throughput log transformation with drastically lower CPU and memory footprints compared to JVM-based Logstash?
19. How do you configure regular expression parsers in Fluentbit (`parsers.conf`), and how do you handle timestamp parsing and runtime type casting (e.g., string to integer)?
20. When an ingestion filter fails (e.g., triggering `_grokparsefailure` or `_jsonparsefailure`), how do you configure Dead Letter Queues (DLQ) to ensure failed events are preserved for inspection?

### Topic 05: Handling Unstructured & Messy Logs

21. What parsing methodologies do you employ to reliably extract security-critical attributes (user IDs, failed actions, target resources) from completely unstructured, free-form developer logs?
22. How do you construct robust parsers that gracefully accommodate variable whitespace delimiters (such as single spaces, tabs, and multiple spaces intermixed) without dropping events?
23. When specific fields are intermittently absent across log events (optional fields / null values), how do you design decoders to prevent extraction failures?
24. How do you properly handle string escaping when log payloads contain special characters, quotation marks (`"`, `'`), backslashes (`\`), or unbalanced delimiters?
25. When unknown or anomalous log formats hit the ingestion pipeline, how do you capture the unparsed payload into a standardized fallback catch-all field (e.g., `log.unparsed`) without dropping the record?

### Topic 06: Key-Value (KV) and Delimited Log Parsing

26. How do you configure automatic key-value parsers to ingest structured appliance logs containing arbitrary key-value pairs (e.g., `src=10.0.0.1 dst=8.8.8.8 proto=TCP action=block`)?
27. When key-value pairs utilize non-standard delimiters (e.g., commas, semicolons, pipes) and values contain quoted whitespace (`msg="User authentication failed"`), how do you configure delimiters to prevent broken tokenization?
28. How do you write field extraction rules using `DELIMS` and `REPORT` within Splunk's `transforms.conf` and `props.conf` configurations?
29. In CSV or tab-delimited log formats, how do you detect and handle schema drift when rows arrive with missing or extra columns to avoid indexing mismatches?
30. What is the standard methodology for parsing dynamic extension key-value pairs from Common Event Format (CEF) and Log Extended Event Format (LEEF) syslog messages?

### Topic 07: Complex & Nested JSON Parsing

31. How do you extract deeply nested attributes (e.g., `userIdentity.arn` or `requestParameters.instancesSet.items[0].instanceId`) from complex multi-level JSON audit logs (such as AWS CloudTrail) using dot notation?
32. Under what operational conditions should arrays of objects within a JSON event be flattened versus exploded (unnested/split into individual independent documents)?
33. How do you resolve indexing mapping conflicts caused by polymorphic JSON fields where an application intermittently emits a field as a string in some events and an integer or object in others?
34. If an adversary injects malformed characters into an application to generate invalid JSON payloads, how do you design fallback handling to prevent parser crashes?
35. How do you configure pipeline pruning rules (`prune` or `remove_field`) to strip high-volume, non-security nested JSON branches before data is written to disk?

### Topic 08: Parsing Latency, Throughput & CPU Profiling

36. How do you profile and measure the exact parsing latency in microseconds per event consumed by a specific decoder or pipeline filter?
37. If activating a new regex parser causes ingestion throughput to plummet from 10,000 EPS to 2,000 EPS, how do you isolate and benchmark the specific regex pattern creating the CPU bottleneck?
38. How do you configure and optimize multi-worker parallel parsing threads in ingestion pipelines to maximize utilization across all available CPU cores?
39. What is the correct sequence of pipeline operations between filtering/dropping and decoding/parsing, and why is early noise suppression computationally superior?
40. What are the performance and garbage-collection advantages of leveraging in-memory lookup caches and pre-compiled regex patterns during high-volume event decoding?

### Topic 09: Decoder Troubleshooting & Debugging Edge Cases

41. When logs from a newly integrated source are ingested but no normalized fields are populated, what step-by-step procedure do you follow to identify which decoder the log is matching?
42. How do you resolve decoder collisions or overlaps when a log message unintentionally matches the prematch criteria of an incorrect decoder, resulting in malformed field extractions?
43. How do you detect and monitor silent decoder degradation when an application software update slightly modifies its log formatting without notice?
44. How do you configure dedicated dashboards and alerting to track parsing failure rates and identify the percentage of daily logs remaining unparsed?
45. How do you safeguard SIEM daemons from crashing due to syntax errors, missing closing XML tags, or invalid regex in user-authored decoder configurations?

### Topic 10: Testing, Quality Assurance & Regression Testing

46. How do you utilize interactive debugging tools like `wazuh-logtest` to feed raw log strings and inspect Phase 1 (pre-decoding), Phase 2 (decoding), and Phase 3 (rule matching)?
47. How do you architect an automated unit-testing suite for custom decoders (e.g., executing test batches of 100+ varied raw logs to validate expected field assertions before deployment)?
48. How do you integrate automated decoder regression testing into CI/CD pipelines (GitLab CI, GitHub Actions) to verify that new decoder commits do not break legacy log parsing?
49. How do you execute offline parsing validation against historical raw log archives to verify field extraction accuracy before promoting rules to production?
50. How do you prepare a comprehensive test evidence package and technical decoder specification document to obtain production sign-off from an Enterprise Security Director?

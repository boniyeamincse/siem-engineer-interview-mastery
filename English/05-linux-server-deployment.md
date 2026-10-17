# 05 — Linux Server Deployment

## SIEM Engineer Interview Questions

### Topic 01: Linux Distribution Selection & Base OS Architecture

1. When choosing between RHEL/Rocky Linux and Ubuntu Server LTS for a production enterprise SIEM (e.g., Wazuh, Elastic, Splunk), what technical factors (kernel stability, long-term support lifecycle, package management, vendor certifications) do you evaluate?
2. Why should a SIEM server always be provisioned using a "Minimal Install," and what security attack surfaces and performance penalties are introduced by unnecessary packages or GUI services?
3. What hardening checklists and benchmarks (e.g., CIS Linux Benchmarks or DISA-STIG) do you follow to harden the base OS of a production SIEM host?
4. How do you verify kernel version compatibility and architecture prerequisites (such as eBPF support, modern NVMe drivers, and advanced networking stacks) during server provisioning?
5. What are the key architectural differences in Linux OS configuration when deploying across Cloud Virtual Machines (AWS EC2, Azure VM), on-prem hypervisors (VMware ESXi / Proxmox), and bare-metal physical servers?

### Topic 02: Storage, Filesystem Architecture & Partitioning

6. How do you design an LVM (Logical Volume Manager) partitioning scheme for a SIEM host, specifically allocating `/var/log`, `/var/ossec` (or data directory), `/tmp`, and `/home`?
7. Why is it mandatory to mount log ingestion and indexer storage directories on separate dedicated filesystems away from the root filesystem (`/`) to prevent OS-level crashes?
8. Between XFS and ext4, which filesystem is optimal for high-throughput SIEM indexing and continuous streaming writes, and why?
9. What are the performance benefits and data integrity risks of using mount options such as `noatime`, `nodiratime`, `barrier=0` (or write-back caching), and `discard`?
10. What is the standard procedure to execute an online volume expansion (live LVM extension and filesystem resize) on a production data partition without downtime?

### Topic 03: Linux Kernel Performance Tuning & Sysctl Optimization

11. Why must the Linux kernel parameter `vm.max_map_count` be increased from its default (65530) to at least 262144 (or higher) prior to running Elasticsearch or OpenSearch indexers, and what memory mapping mechanism does it govern?
12. Why is the Linux kernel parameter `vm.swappiness` lowered to 1 (or 0) on SIEM indexers, and how does memory swapping trigger catastrophic Java Garbage Collection (GC) pauses?
13. How do you tune dirty memory write-back thresholds (`vm.dirty_background_ratio` and `vm.dirty_ratio`) to prevent system-wide I/O stalls during high-volume data writes?
14. What role does `fs.file-max` play in increasing system-wide file descriptor allocations, and how is its optimal value calculated?
15. What is the configuration management best practice for managing persistent kernel parameters under `/etc/sysctl.d/` across reboots?

### Topic 04: Process Limits, Systemd & Resource Management

16. Why is it essential to tune `nofile` (maximum open files, e.g., 65536 or higher) and `nproc` (maximum processes) in `/etc/security/limits.conf` for SIEM service accounts?
17. Since systemd services often ignore `limits.conf` defaults, how do you properly configure `LimitNOFILE=65536` and `LimitNPROC=4096` inside systemd service unit files?
18. What is the purpose of setting `LimitMEMLOCK=infinity` alongside `bootstrap.memory_lock: true` in the application configuration, and how do you verify that memory locking is active?
19. How do you leverage systemd Cgroups (Control Groups) to establish resource constraints (`CPUQuota`, `MemoryLimit`) preventing a misbehaving custom script or parser from exhausting host resources?
20. What is the recommended architectural practice between modifying `/etc/systemd/system.conf` directly versus implementing systemd drop-in override files (`override.conf`) under `/etc/systemd/system/`?

### Topic 05: Network Stack Tuning & High-Throughput Socket Optimization

21. How do you tune Linux socket receive buffers (`net.core.rmem_max`, `net.core.rmem_default`) and send buffers to handle thousands of incoming Syslog (UDP/TCP) packets per second?
22. Why should the kernel network input queue `net.core.netdev_max_backlog` and connection backlog `net.core.somaxconn` be increased from the default 128 to 1024 or 4096 to prevent socket drops during sudden bursts?
23. How do you configure TCP parameter tuning (`net.ipv4.tcp_tw_reuse`, `net.ipv4.tcp_fin_timeout`, and TCP keepalive settings) to accelerate connection recycling and reduce TIME_WAIT sockets?
24. How do you utilize `ethtool` to maximize network interface card (NIC) RX/TX ring buffer parameters to eliminate packet drops at the driver level?
25. Under what architectural conditions should Jumbo Frames (MTU 9000) be enabled for high-volume ingestion, and how do you validate end-to-end MTU consistency across switches and routers?

### Topic 06: Time Synchronization, PTP & Chrony Engineering

26. Why is sub-millisecond time synchronization critical across a SIEM environment, and how do clock discrepancies corrupt event correlation rules and causality chains?
27. Why is `chrony` preferred over legacy `ntpd` for rapid, highly stable clock synchronization on modern Linux enterprise servers?
28. How do you configure a fault-tolerant `/etc/chrony.conf` pointing to redundant internal enterprise Stratum 1 and Stratum 2 NTP server pools?
29. How should leap seconds be handled by the kernel clock, and how does SIEM ingestion interact with cloud provider "Leap Smearing" strategies?
30. How do you monitor clock drift and jitter using `chronyc sources -v`, `chronyc tracking`, and syslog alerts to trigger automated alerting before drift impacts correlation?

### Topic 07: Linux Firewalling, Network Security & Interface Isolation

31. How do you implement a strict "Default Deny All" inbound firewall policy on production SIEM hosts using `firewalld` or `nftables`?
32. What is the standard methodology for isolating management traffic (SSH, Web UI, API) from ingestion traffic (Syslog 514, Agent 1514, Logstash 5044) across distinct physical/VLAN interfaces with source IP whitelisting?
33. What specific kernel and firewall configurations are required to prevent raw Syslog (UDP port 514) packets from being silently dropped under heavy ingest loads?
34. How do you configure firewall rate-limiting and connection throttling to protect SIEM listeners against SYN floods and unauthorized port scanning?
35. How do you configure egress filtering on SIEM servers to block all unauthorized outbound connections, permitting only vetted Threat Intel feeds and official vendor update repositories?

### Topic 08: OS-Level Security, SELinux / AppArmor & Least Privilege

36. Why should SELinux never be set to `disabled` or `permissive` on an enterprise SIEM host, and how do you maintain a production SIEM in `Enforcing` mode?
37. If SIEM components store logs or indices in non-standard paths (e.g., `/data/siem_logs` or `/data/indexer`), how do you apply the appropriate SELinux file contexts using `semanage fcontext` and `restorecon`?
38. When binding SIEM services or APIs to non-standard ports, how do you define and allow SELinux port contexts via `semanage port -a -t ...`?
39. Why must SIEM services (Wazuh, Logstash, Elasticsearch) never execute as the `root` user, and how do you enforce least-privilege daemon execution via dedicated system service accounts?
40. How do you configure `auditd` to monitor privileged admin command execution and file integrity on critical system paths (`/etc/passwd`, `/etc/shadow`, `/etc/sysctl.conf`, `/etc/ssh/sshd_config`)?

### Topic 09: Storage Performance Validation & Baseline Testing

41. Prior to installing SIEM software, how do you execute standardized storage benchmarking scripts using `fio` to measure true Random Write IOPS, Sequential Write throughput, and I/O latency?
42. How do you utilize 4K Random Write and 1MB Sequential Write benchmark tests to determine whether underlying storage meets the demands of indexing and log archiving?
43. How do you diagnose disk controller saturation and buffer bottlenecks by monitoring `%util`, `await`, and `r/s, w/s` using `iostat -xz 1` and `ioping`?
44. On virtualized or cloud block storage, how do you detect underlying hypervisor I/O throttling or burst credit exhaustion before going live?
45. How do you assess disk health using `smartctl` (evaluating bad sectors, wear-leveling counts) and verify the RAID controller's Battery Backup Unit (BBU) write-cache status?

### Topic 10: Server Automation, Golden Images & Post-Deployment Validation

46. How do you codify the entire Linux OS deployment, tuning, and hardening process into Infrastructure-as-Code (IaC) using Ansible Playbooks, Terraform, and Cloud-Init?
47. How do you build an automated pipeline using HashiCorp Packer to bake CIS-hardened "Golden Images" for rapid, consistent SIEM server provisioning?
48. Following OS deployment, how do you execute an automated post-deployment validation script (Bash or Python) to verify that kernel parameters, file limits, firewall rules, and chrony configurations meet specifications?
49. What is your rollback strategy and snapshot lifecycle policy to revert the server to a known good state if deployment or configuration automation fails?
50. What documented evidence, benchmark metrics, and security checklists must be compiled to obtain sign-off certifying a server as "Production-Ready for SIEM"?

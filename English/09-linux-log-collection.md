# 09 — Linux Log Collection

## SIEM Engineer Interview Questions

### Topic 01: Linux Logging Ecosystem & Subsystem Architecture

1. What are the architectural and data collection differences among the various Linux logging subsystems (Classic Syslog, Systemd Journald, Linux Audit Subsystem / Auditd, and modern eBPF)?
2. How do you accommodate differences in log file organization between Debian/Ubuntu (`/var/log/auth.log`, `/var/log/syslog`) and RHEL/Rocky Linux (`/var/log/secure`, `/var/log/messages`) within SIEM endpoint agent collection profiles?
3. When a new process is executed or a file is accessed on Linux, what is the end-to-end data flow from kernel space through `auditd` or `journald` to the SIEM agent?
4. On a high-traffic Linux production server (e.g., a heavily loaded Nginx web cluster or database), how do you ensure the system logging services themselves do not induce excessive CPU and memory consumption?
5. When modern Linux distributions deprecate traditional flat-file logging in favor of systemd binary journals (`journald`), how do you configure direct journal reading performance for SIEM endpoint agents?

### Topic 02: Authentication, Remote Access & PAM Auditing

6. Which log patterns and event indicators must be monitored to comprehensively audit SSH logins (successful and failed), public key authentications, and password-based logons on Linux servers?
7. How does the Pluggable Authentication Modules (PAM) framework operate, and how do you configure `/etc/pam.d/` to capture detailed logs for authentication failures and access denial events?
8. What parsing strategy should be applied to extract source IP addresses, destination ports, and invalid usernames from authentication logs to detect SSH brute-force attacks and slow password spraying?
9. How do you distinguish between local console logons (TTY), remote SSH sessions, and graphical desktop logons across Linux audit trails?
10. How do you detect unauthorized SSH key modifications, additions, or permission changes in `authorized_keys` files in real time using File Integrity Monitoring (FIM) and the audit subsystem?

### Topic 03: Linux Auditd Subsystem & Ruleset Engineering

11. Why is the Linux Audit daemon (`auditd`) substantially more robust for security monitoring than standard syslog, and how does it inspect events at the kernel system call (syscall) level?
12. How do you author a performance-optimized audit rule in `/etc/audit/rules.d/audit.rules` to capture critical program executions via the `execve` and `execveat` syscalls?
13. Demonstrate the proper configuration for file and directory integrity watches (e.g., `/etc/passwd`, `/etc/shadow`, `/etc/sudoers`, `/bin/crontab`) with appropriate read/write/attribute filters and audit keys (`-k key_name`).
14. How do you tune `backlog_limit`, `backlog_wait_time`, and failure modes (`-f 1` vs. `-f 2`) in `auditd.conf` to prevent system freezes or panics when audit event buffers become saturated on high-load servers?
15. What audit rule syntax should be implemented to suppress benign high-frequency noise (such as routine system cron jobs or SIEM monitoring agent syscalls) at the kernel level?

### Topic 04: Privileged Escalation, Sudo & User Command Logging

16. What configuration options must be enabled in `/etc/sudoers` to ensure that full command-line arguments, working directories, and target user contexts are logged when a standard user executes `sudo` commands?
17. If a user spawns an interactive root shell via `sudo su -` or `sudo -i`, standard `sudo` logging fails to capture subsequent commands; how do you resolve this visibility gap using PAM TTY auditing (`pam_tty_audit.so`)?
18. If a malicious insider attempts to evade detection by disabling shell history (`unset HISTFILE`, `kill -9 $$`, or invoking `sh`), how does the Linux audit subsystem ensure that every executed command is indelibly captured?
19. How do you design audit rules to track the execution of SUID/SGID binaries (e.g., `/usr/bin/passwd`, `/usr/bin/gpasswd`, or newly dropped malicious SUID backdoors) to identify privilege escalation attempts?
20. What mechanisms are used to monitor Linux Capability modifications (e.g., `setuid`, `net_admin`, `sys_admin`) and inter-process injection attempts via the `ptrace` system call?

### Topic 05: Systemd Journald Integration & Forwarding

21. How does the binary log architecture of Systemd Journald function, and what is the operational impact of setting the storage type to `persistent` versus `volatile` in `/etc/systemd/journald.conf`?
22. How do you optimize `RateLimitIntervalSec` and `RateLimitBurst` parameters in `journald.conf` to prevent journald from dropping messages during high-volume log bursts?
23. How do SIEM endpoint agents read journal metadata (e.g., `_SYSTEMD_UNIT`, `_PID`, `_COMM`, `_UID`) directly from `/run/systemd/journal/` or `/var/log/journal/` sockets?
24. How do you configure real-time streaming of systemd unit state changes, service crashes, and service restart failures to the SIEM?
25. How do you manage journal log disk consumption using `SystemMaxUse`, `SystemKeepFree`, and `MaxRetentionSec` directives to prevent disk exhaustion?

### Topic 06: Kernel Logs, Hardware Events & Out-Of-Memory (OOM)

26. What is the technical difference between the Linux kernel ring buffer (`dmesg`) and persistent kernel log channels (`/var/log/kern.log` or `/var/log/messages`), and how can SIEM agents stream kernel events in real time?
27. When the Linux kernel Out-Of-Memory (OOM) killer terminates a critical security service or application process (`invoked oom-killer: gfp_mask=...`), how do you immediately detect and alert on this event?
28. Which log patterns and signatures indicate kernel panics, segmentation faults (`segfault`), driver crashes, and hardware storage I/O read/write errors?
29. What audit configuration is required to monitor the loading of new kernel modules or rootkits via the `init_module` and `finit_module` system calls?
30. How are system clock alterations (`settimeofday`, `clock_settime` syscalls) or network interfaces entering promiscuous mode (indicative of packet sniffing) detected in Linux logs?

### Topic 07: Container & Kubernetes Node Logging

31. How do you configure an endpoint SIEM agent to collect containerized stdout/stderr logs directly from Docker or container runtime host storage (`/var/lib/docker/containers/*/*.log`)?
32. What are the trade-offs of different container log drivers (e.g., `json-file`, `journald`, `fluentd`), and how do you mitigate host disk exhaustion risks caused by unrotated container logs?
33. On Kubernetes worker nodes, how does pod log collection work with Container Runtime Interfaces (CRI - containerd or CRI-O), and how are logs enriched with pod name, namespace, and container ID metadata?
34. How can the host Linux audit subsystem detect container breakout attempts or unauthorized attempts to gain root privileges on the underlying host OS from within a container?
35. What is the strategy for auditing container engine operations and Docker daemon socket interactions (e.g., `docker exec` sessions, privileged container creation, stop/start events)?

### Topic 08: Linux Log File Management, Logrotate & Filtering

36. When Linux `logrotate` compresses or renames active log files (e.g., rotating `auth.log` to `auth.log.1`), how does a SIEM agent track file pointers and maintain continuous, zero-loss log ingestion?
37. What are the comparative advantages and operational risks of using `copytruncate` versus `create` directives in `logrotate` configurations, and how do they affect the agent's inode tracking?
38. How do you handle multiline log entries (such as Java stack traces or Python exception traces) during Linux log file tailing to ensure they are ingested as cohesive, single events?
39. How do you construct regular expression exclusion filters at the host agent level to drop non-security, high-frequency log noise (e.g., local health checks executing every minute) before network transmission?
40. How do you diagnose and measure ingestion latency caused by discrepancies between Linux disk write throughput and the agent's file reading/processing rate?

### Topic 09: Tampering Detection & Log Anti-Forensics Defense

41. If an adversary compromises a Linux server and executes an anti-forensic log wipe (`rm -rf /var/log/*`), how can a SIEM immediately detect and alert on this tampering activity?
42. What mechanisms should be established to alert on or prevent the suspension of the audit daemon (`systemctl stop auditd`) or the flushing of kernel audit rules (`auditctl -D`)?
43. How do you leverage Linux file attributes (`chattr +a /var/log/audit/audit.log` for append-only, and `chattr +i` for immutable) to protect critical audit logs, and what are the limitations of this control?
44. How can you detect and trigger alerts if an attacker truncates a log file by overwriting it with zero bytes (`cat /dev/null > /var/log/secure`)?
45. Why is real-time, off-host log forwarding from Linux endpoints to a hardened remote SIEM the most resilient defense against local anti-forensic tampering?

### Topic 10: Linux Log Validation, Red Team Simulation & Testing

46. How do you design and execute a controlled validation test using Atomic Red Team or manual bash scripts to verify the end-to-end functionality of the Linux log ingestion pipeline?
47. How do you simulate a reverse shell (`bash -i >& /dev/tcp/...`), local account creation (`useradd`), and cron persistence (`/etc/cron.d/`) on a test endpoint, and verify each stage appears accurately in the SIEM?
48. Demonstrate the command syntax for injecting test syslog messages across various facility and severity combinations using the `logger` utility (e.g., `logger -p auth.alert "Simulated Security Incident"`).
49. How do you ensure that kernel audit rules persist across Linux system reboots, and how do you programmatically validate their active operational state?
50. How do you compile a comprehensive test evidence document and logging readiness audit report demonstrating 100% compliance across 50+ enterprise Linux production servers for a Security Architect?

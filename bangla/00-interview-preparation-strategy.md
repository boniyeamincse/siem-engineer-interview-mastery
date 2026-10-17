# 00 — SIEM Engineer Interview Questions

## ধাপ ০১: Requirement Assessment & Planning

১. একটি প্রতিষ্ঠানে ৫০০টি Windows ও Linux endpoint, Firewall, VPN, Switch এবং Critical Server রয়েছে। SIEM Engineer হিসেবে SIEM deployment শুরু করার আগে কী কী requirement সংগ্রহ করবে?

২. প্রতিষ্ঠানের asset inventory, log source, business-critical system, compliance requirement এবং log retention requirement কীভাবে নির্ধারণ করবে?

৩. SIEM deployment-এর আগে কীভাবে বুঝবে প্রতিষ্ঠানের জন্য কোন ধরনের architecture এবং infrastructure প্রয়োজন?

## ধাপ ০২: SIEM Architecture Design

৪. ৫০০টি endpoint ও বিভিন্ন network device-এর জন্য শুরু থেকে একটি enterprise SIEM architecture কীভাবে design করবে?

৫. SIEM Manager, Indexer, Dashboard, Agent এবং Syslog Collector-এর মধ্যে communication architecture কীভাবে তৈরি করবে?

৬. একটি প্রতিষ্ঠানের বিভিন্ন VLAN, subnet, data center এবং remote office থেকে log সংগ্রহের architecture কীভাবে design করবে?

৭. Centralized ও distributed SIEM architecture-এর মধ্যে কোনটি নির্বাচন করবে এবং কীভাবে সেই সিদ্ধান্ত নেবে?

## ধাপ ০৩: SIEM Platform Selection

৮. Wazuh, Splunk, Elastic SIEM এবং Microsoft Sentinel-এর মধ্যে একটি প্রতিষ্ঠানের জন্য উপযুক্ত platform কীভাবে নির্বাচন করবে?

৯. SIEM platform নির্বাচন করার সময় licensing cost, infrastructure requirement, scalability, integration এবং maintenance কীভাবে মূল্যায়ন করবে?

১০. একটি প্রতিষ্ঠানের বর্তমান SIEM platform প্রয়োজনীয় workload সামলাতে না পারলে migration অথবা platform replacement কীভাবে পরিকল্পনা করবে?

## ধাপ ০৪: Infrastructure Sizing & Capacity Planning

১১. ৫০০টি endpoint-এর জন্য SIEM server-এর CPU, RAM, Storage, IOPS এবং Network Bandwidth কীভাবে হিসাব করবে?

১২. একটি SIEM environment-এ Average EPS ও Peak EPS কীভাবে নির্ধারণ করবে এবং এই তথ্য infrastructure sizing-এ কীভাবে ব্যবহার করবে?

১৩. প্রতিদিন ৫০ GB log ingest হলে ৯০ দিনের retention-এর জন্য মোট কত storage প্রয়োজন হবে? Indexing overhead, replicas এবং free-space reserve কীভাবে হিসাব করবে?

১৪. একটি SIEM environment-এর log volume ও endpoint সংখ্যা কয়েক গুণ বেড়ে গেলে কীভাবে capacity পুনর্মূল্যায়ন করবে?

## ধাপ ০৫: SIEM Installation & Configuration

১৫. Production environment-এ SIEM install করার আগে Linux server-এর OS, disk, filesystem, network এবং security configuration কীভাবে প্রস্তুত করবে?

১৬. SIEM Manager, Indexer ও Dashboard আলাদা server-এ deploy করতে হলে installation sequence এবং component configuration কীভাবে নির্ধারণ করবে?

১৭. SIEM installation-এর পরে service health, API connectivity, indexing, search functionality এবং dashboard connectivity কীভাবে validate করবে?

## ধাপ ০৬: Endpoint Agent Deployment

১৮. ৫০০টি Windows ও Linux endpoint-এ SIEM agent কীভাবে deploy, enroll, configure এবং centrally manage করবে?

১৯. Active Directory Group Policy, software deployment tools অথবা Ansible ব্যবহার করে agent deployment কীভাবে automate করবে?

২০. কোনো endpoint-এ agent install হয়েছে, কিন্তু SIEM-এর সঙ্গে connect হচ্ছে না—এ অবস্থায় কোন কোন configuration ও communication path পরীক্ষা করবে?

২১. কীভাবে নিশ্চিত করবে যে inventory-তে থাকা সব endpoint সঠিক agent configuration-সহ SIEM-এ onboard হয়েছে?

## ধাপ ০৭: Log Collection & Integration

২২. Windows Security, System, PowerShell এবং Sysmon logs সংগ্রহের জন্য কী configuration প্রয়োজন হবে?

২৩. Linux server থেকে authentication, sudo, auditd, journald এবং application logs সংগ্রহের ব্যবস্থা কীভাবে করবে?

২৪. Firewall, VPN, Switch এবং IDS/IPS থেকে Syslog collection configure করার সময় কোন কোন বিষয় বিবেচনা করবে?

২৫. একটি custom application JSON format-এ log তৈরি করলে সেটিকে SIEM-এ ingest এবং searchable করার জন্য কী কী configuration প্রয়োজন হবে?

## ধাপ ০৮: Log Parsing & Normalization

২৬. একটি নতুন log source থেকে log আসছে, কিন্তু প্রয়োজনীয় fields extract হচ্ছে না—decoder, parser এবং field mapping কীভাবে configure ও troubleshoot করবে?

২৭. Windows, Linux এবং Network Device-এর timestamp ও timezone আলাদা হলে event normalization এবং consistent searching কীভাবে নিশ্চিত করবে?

২৮. SIEM-এ duplicate logs, missing logs অথবা ingestion delay দেখা দিলে source থেকে indexer পর্যন্ত কোন কোন component পরীক্ষা করবে?

## ধাপ ০৯: Detection Engineering

২৯. একটি নতুন detection requirement পাওয়ার পর log source নির্বাচন, detection logic, rule configuration, severity এবং threshold কীভাবে নির্ধারণ করবে?

৩০. Custom detection rule production-এ deploy করার আগে কীভাবে test, validate এবং regression testing করবে?

৩১. একটি নতুন rule deploy করার পর false-positive alert বেড়ে গেলে কীভাবে rule tuning এবং validation করবে?

৩২. Custom rules ও decoders-এর version control, change review, testing এবং rollback কীভাবে পরিচালনা করবে?

৩৩. Detection rules-কে MITRE ATT&CK techniques-এর সঙ্গে কীভাবে map করবে এবং detection coverage কীভাবে যাচাই করবে?

## ধাপ ১০: Log Pipeline & Performance Testing

৩৪. একটি log source থেকে শুরু করে collector, processing, indexing এবং search পর্যন্ত end-to-end pipeline কীভাবে test করবে?

৩৫. SIEM-এ EPS হঠাৎ বেড়ে গেলে processing throughput, queue backlog, indexing latency, CPU, RAM এবং disk I/O কীভাবে পরীক্ষা করবে?

৩৬. Log ingestion স্বাভাবিক থাকলেও search ধীর হলে সম্ভাব্য bottleneck কীভাবে শনাক্ত করবে?

৩৭. Production deployment-এর আগে SIEM-এর load testing, stress testing এবং performance benchmarking কীভাবে করবে?

## ধাপ ১১: Dashboard & Alerting Engineering

৩৮. SIEM infrastructure-এর জন্য agent health, ingestion rate, queue backlog, storage utilization এবং service availability dashboard কীভাবে তৈরি করবে?

৩৯. Log pipeline failure, agent disconnection, disk saturation এবং indexer failure শনাক্ত করার জন্য monitoring ও automated alerting কীভাবে configure করবে?

৪০. SIEM platform-এর নিজস্ব health monitoring কীভাবে design করবে, যাতে SIEM-এর monitoring component ব্যর্থ হলেও সমস্যাটি শনাক্ত করা যায়?

## ধাপ ১২: High Availability & Disaster Recovery

৪১. SIEM Manager, Indexer অথবা Syslog Collector down হলে monitoring continuity বজায় রাখার জন্য কীভাবে High Availability architecture design করবে?

৪২. SIEM server সাময়িকভাবে unavailable হলে incoming logs কীভাবে buffer বা queue করবে, যাতে log loss-এর ঝুঁকি কমে?

৪৩. সম্পূর্ণ SIEM server বা storage failure হলে backup থেকে recovery কীভাবে করবে?

৪৪. SIEM infrastructure-এর জন্য RTO ও RPO কীভাবে নির্ধারণ করবে এবং Disaster Recovery test কীভাবে পরিচালনা করবে?

## ধাপ ১৩: Security Hardening

৪৫. Production SIEM-এর জন্য RBAC, MFA, TLS, least privilege এবং management network segmentation কীভাবে configure করবে?

৪৬. SIEM API, service account, certificates, credentials এবং secrets কীভাবে নিরাপদে পরিচালনা করবে?

৪৭. SIEM configuration পরিবর্তন, administrative activity এবং access attempts-এর audit logging কীভাবে নিশ্চিত করবে?

৪৮. SIEM server, operating system এবং সংশ্লিষ্ট components-এর security patching ও vulnerability management কীভাবে পরিচালনা করবে?

## ধাপ ১৪: Storage & Retention Management

৪৯. SIEM-এর storage utilization দ্রুত বাড়তে থাকলে কীভাবে ingestion volume, indexing overhead, retention policy এবং disk usage বিশ্লেষণ করবে?

৫০. Hot, warm ও cold storage অথবা archival strategy ব্যবহার করে দীর্ঘমেয়াদি log retention কীভাবে design করবে?

৫১. Retention period শেষ হলে প্রয়োজনীয় policy অনুসারে log lifecycle, archival এবং deletion কীভাবে পরিচালনা করবে?

## ধাপ ১৫: Scaling & Optimization

৫২. ৫০০ endpoint থেকে ৫,০০০ endpoint-এ উন্নীত হলে SIEM architecture কীভাবে scale করবে?

৫৩. দৈনিক log ingestion ৫০ GB থেকে ৫০০ GB-এ বেড়ে গেলে processing, storage এবং indexing capacity কীভাবে বাড়াবে?

৫৪. SIEM infrastructure-এর resource utilization, indexing performance এবং search performance নিয়মিত optimize করার পদ্ধতি কী হবে?

৫৫. Infrastructure cost কমানোর সময় detection quality, log completeness এবং retention requirement কীভাবে বজায় রাখবে?

## ধাপ ১৬: Upgrade & Change Management

৫৬. Production SIEM-এ version upgrade করার আগে কী কী compatibility check, backup এবং pre-upgrade test করবে?

৫৭. SIEM agent, manager, indexer এবং dashboard upgrade-এর জন্য deployment sequence ও rollback plan কীভাবে তৈরি করবে?

৫৮. নতুন decoder, detection rule অথবা configuration production-এ release করার আগে কীভাবে change approval ও validation করবে?

৫৯. SIEM upgrade-এর পরে log ingestion, indexing, detection rules এবং dashboard সঠিকভাবে কাজ করছে কি না কীভাবে verify করবে?

## ধাপ ১৭: Troubleshooting & Production Engineering

৬০. SIEM-এ হঠাৎ log ingestion বন্ধ হয়ে গেলে source, network, collector, processing এবং storage layer ধরে কীভাবে troubleshooting করবে?

৬১. SIEM Indexer-এর disk প্রায় পূর্ণ হয়ে গেলে data ingestion এবং service availability বজায় রাখার জন্য কী পদক্ষেপ নেবে?

৬২. একটি SIEM component বারবার restart হলে logs, resource metrics, service dependencies এবং configuration পরীক্ষা করে root cause কীভাবে নির্ধারণ করবে?

৬৩. SIEM-এর কোনো configuration পরিবর্তনের পর পুরো pipeline অকার্যকর হয়ে গেলে কীভাবে rollback এবং recovery করবে?

## ধাপ ১৮: End-to-End Enterprise SIEM Project

৬৪. ৫০০টি Windows ও Linux endpoint, Firewall, VPN, Switch এবং Critical Server-সহ একটি প্রতিষ্ঠানের সম্পূর্ণ SIEM solution design, deploy, configure, test এবং maintain করার পরিকল্পনা কী হবে?

৬৫. Production deployment সম্পন্ন হয়েছে বলে ঘোষণা করার আগে কোন কোন acceptance criteria, test evidence, documentation এবং operational readiness যাচাই করবে?

৬৬. SIEM deployment শেষ হওয়ার পরে ongoing maintenance, capacity review, configuration management, performance monitoring এবং continuous improvement কীভাবে পরিচালনা করবে?

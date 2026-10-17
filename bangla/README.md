# SIEM Engineer Interview Mastery (বাংলা সংস্করণ)

**SIEM ইঞ্জিনিয়ারিং, আর্কিটেকচার ডিজাইন, ডিপ্লয়মেন্ট, টেস্টিং এবং প্রোডাকশন অপারেশন্সের জন্য একটি সিনারিও-ভিত্তিক লার্নিং ও ইন্টারভিউ রোডম্যাপ**

[![Focus](https://img.shields.io/badge/Focus-SIEM%20Engineering-blue)](https://github.com/)
[![Learning](https://img.shields.io/badge/Approach-Scenario--Based-success)](https://github.com/)
[![Level](https://img.shields.io/badge/Level-Beginner%20to%20Advanced-orange)](https://github.com/)
[![Language](https://img.shields.io/badge/Language-Bengali%20%2F%20Bangla-green)](https://github.com/)
[![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)](https://github.com/)

---

## 📌 এই রিপোজিটরি সম্পর্কে

এই রিপোজিটরিটি মূলত সেইসব ইঞ্জিনিয়ারদের জন্য তৈরি করা হয়েছে যারা মুখস্থ উত্তর দেওয়ার পরিবর্তে বাস্তব **Enterprise Production Environment**-এর জটিল সমস্যা সমাধান করতে চান এবং একজন দক্ষ **SIEM Engineer** হিসেবে নিজেকে প্রতিষ্ঠা করতে চান।

এখানে শুধু সাধারণ ইন্টারভিউ প্রশ্নোত্তর দেওয়া হয়নি; বরং বাস্তব এন্টারপ্রাইজ সিনারিও, আর্কিটেকচারাল সিদ্ধান্ত, ক্যাপাসিটি ক্যালকুলেশন, কম্পোনেন্ট কনফিগারেশন, লগ পাইপলাইন ভ্যালিডেশন, ডিটেকশন ইঞ্জিনিয়ারিং এবং প্রোডাকশন ট্রাবলশুটিং-এর উপর ভিত্তি করে প্রতিটি বিষয় সাজানো হয়েছে।

---

## 🎯 লার্নিং অবজেক্টিভস (লক্ষ্যসমূহ)

এই রোডম্যাপটি সম্পন্ন করার মাধ্যমে একজন ইঞ্জিনিয়ার যা যা শিখবেন এবং প্রদর্শন করতে পারবেন:

1. **Architecture & Design:** বিজনেস ও সিকিউরিটি রিকোয়ারমেন্টের ভিত্তিতে এন্টারপ্রাইজ SIEM আর্কিটেকচার ডিজাইন করা।
2. **Capacity Planning & Sizing:** EPS, GB/day, ইনডেক্সিং ওভারহেড, রিটেনশন পলিসি ও স্টোরেজ গ্রোথের সঠিক গাণিতিক হিসাব।
3. **Infrastructure Deployment:** লিনাক্স সার্ভার প্রিপারেশন, কার্নেল অপ্টিমাইজেশন এবং SIEM কম্পোনেন্ট (ম্যানেজার, ইনডেক্সার, ড্যাশবোর্ড, কালেক্টর) ডিপ্লয়মেন্ট।
4. **Log Collection at Scale:** Windows (Event Logs, Sysmon, PowerShell), Linux (Auditd, Journald, Auth), নেটওয়ার্ক ডিভাইস (Firewall, Switch, VPN) ও কাস্টম অ্যাপ্লিকেশনের লগ সংগ্রহ।
5. **Log Engineering:** কাস্টম লগ পার্সিং, ডিকোডার তৈরি, ফিল্ড ম্যাপিং ও টাইমস্ট্যাম্প নরমালাইজেশন।
6. **Data Quality & Validation:** লগ সোর্স থেকে ইনডেক্সার পর্যন্ত এন্ড-টু-এন্ড পাইপলাইন ভ্যালিডেশন এবং ডেটা ইন্টিগ্রিটি নিশ্চিতকরণ।
7. **Detection Engineering:** থ্রেট মডেলিং, ডিটেকশন রুল তৈরি, পজিটিভ/নেগেটিভ টেস্টিং, ফলস-পজিটিভ টিউনিং এবং MITRE ATT&CK ম্যাপিং।
8. **Rule CI/CD:** গিট-ভিত্তিক রুল ম্যানেজমেন্ট, কোড রিভিউ, অটোমেটেড ভ্যালিডেশন এবং রোলব্যাক মেকানিজম।
9. **High Availability & DR:** ক্লাস্টারিং, লোড ব্যালান্সিং, মেসেজ বাফারিং (Queueing), ব্যাকআপ এবং Disaster Recovery প্ল্যানিং।
10. **Security Hardening:** RBAC, MFA, TLS এনক্রিপশন, সিক্রেট ম্যানেজমেন্ট এবং SIEM-এর নিজস্ব অডিট ট্রেইল নিশ্চিতকরণ।
11. **Self-Monitoring & Troubleshooting:** এজেন্ট হেলথ, ইনজেশন ড্রপ, কিউ ব্যাকলগ, ডিস্ক স্যাচুরেশন এবং প্রোডাকশন বিভ্রাট সমাধান করা।
12. **Interview Articulation:** টেকনিক্যাল ইন্টারভিউতে প্রতিটি ইঞ্জিনিয়ারিং সিদ্ধান্তের পক্ষে যুক্তিযুক্ত ও প্রফেশনাল ব্যাখ্যা উপস্থাপন করা।

---

## 👥 এই রিপোজিটরি কাদের জন্য?

- 🔹 **Aspiring SIEM Engineers** — যারা এন্টারপ্রাইজ লগ ও ডিটেকশন ইনফ্রাস্ট্রাকচারে ক্যারিয়ার গড়তে চান।
- 🔹 **Junior / Mid-Level SIEM Engineers** — যারা ডিপ্লয়মেন্ট, স্কেলিং ও রুল ইঞ্জিনিয়ারিং-এ দক্ষতা গভীর করতে চান।
- 🔹 **SOC Analysts** — যারা অ্যালার্ট ট্রায়াজ থেকে ইনফ্রাস্ট্রাকচার ও ডিটেকশন ইঞ্জিনিয়ারিং রোলে ট্রানজিশন করতে চান।
- 🔹 **Security Engineers / SysAdmins** — যারা প্রতিষ্ঠানের সেন্ট্রালাইজড লগিং ও অডিটিং আর্কিটেকচারের দায়িত্বে রয়েছেন।
- 🔹 **Interview Candidates** — যারা সিনিয়র ও এন্টারপ্রাইজ-লেভেল SIEM ইন্টারভিউয়ের জন্য প্রস্তুতি নিচ্ছেন।

> [!NOTE]
> এটি কোনো সাধারণ L1 SOC অ্যালার্ট মনিটরিং প্রশ্নব্যাংক নয়। এর প্রধান ফোকাস হলো **ইঞ্জিনিয়ারিং, সিস্টেম ডিজাইন, লগ পাইপলাইন, স্কেলেবিলিটি ও প্রোডাকশন অপারেশনস**।

---

## 🗺️ লার্নিং রোডম্যাপ ও ফাইল ইনডেক্স

পুরো প্রস্তুতিটি ৬টি ফেজে বিভক্ত এবং মোট ৩১টি মডিউলে সাজানো হয়েছে:

### Phase 01 — Architecture and Planning (পরিকল্পনা ও আর্কিটেকচার)
| ফাইল | বিষয়ের বিবরণ |
|---|---|
| [`00-interview-preparation-strategy.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/00-interview-preparation-strategy.md) | ১৮টি ধাপে ৬৬টি মাস্টার রিয়েল-ওয়ার্ল্ড ইন্টারভিউ প্রশ্ন ও পূর্ণাঙ্গ প্রস্তুতি গাইড |
| [`01-requirement-assessment.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/01-requirement-assessment.md) | বিজনেস রিকোয়ারমেন্ট, অ্যাসেট ইনভেন্টরি, লগ সোর্স, রিটেনশন ও কমপ্লায়েন্স অ্যাসেসমেন্ট |
| [`02-siem-architecture-design.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/02-siem-architecture-design.md) | এন্টারপ্রাইজ সেন্ট্রালাইজড, ডিস্ট্রিবিউটেড ও হাইব্রিড আর্কিটেকচার ডিজাইন এবং ডেটা ফ্লো |
| [`03-platform-selection.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/03-platform-selection.md) | Wazuh, Splunk, Elastic SIEM এবং Microsoft Sentinel-এর তুলনামূলক ইঞ্জিনিয়ারিং মূল্যায়ন |
| [`04-infrastructure-sizing.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/04-infrastructure-sizing.md) | EPS, GB/day, CPU, RAM, IOPS, নেটওয়ার্ক ব্যান্ডউইথ এবং স্টোরেজ ওভারহেড ক্যালকুলেশন |

### Phase 02 — Infrastructure Deployment (ইনফ্রাস্ট্রাকচার ডিপ্লয়মেন্ট)
| ফাইল | বিষয়ের বিবরণ |
|---|---|
| [`05-linux-server-deployment.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/05-linux-server-deployment.md) | লিনাক্স ওএস প্রিপারেশন, ফাইলসিস্টেম, কার্নেল টিউনিং ও সার্ভার ডিপ্লয়মেন্ট |
| [`06-siem-component-configuration.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/06-siem-component-configuration.md) | ম্যানেজার, ইনডেক্সার, ড্যাশবোর্ড এবং ডেডিকেটেড কালেক্টরের কনফিগারেশন ও ক্লাস্টারিং |

### Phase 03 — Endpoint and Log Integration (এন্ডপয়েন্ট ও লগ ইন্টিগ্রেশন)
| ফাইল | বিষয়ের বিবরণ |
|---|---|
| [`07-endpoint-agent-deployment.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/07-endpoint-agent-deployment.md) | এজেন্ট ইনস্টলেশন, এনরোলমেন্ট, অটোমেশন (GPO/Ansible) এবং সেন্ট্রাল ম্যানেজমেন্ট |
| [`08-windows-log-collection.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/08-windows-log-collection.md) | Windows Security, System Event Log, PowerShell অডিটিং এবং Sysmon ইন্টিগ্রেশন |
| [`09-linux-log-collection.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/09-linux-log-collection.md) | Authentication logs, Sudo, Auditd রুলস এবং Journald লগ কালেকশন |
| [`10-network-device-integration.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/10-network-device-integration.md) | Firewall, VPN, Switch, Router এবং IDS/IPS থেকে Syslog কালেকশন ও সিকিউর ফরোয়ার্ডিং |
| [`11-application-log-integration.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/11-application-log-integration.md) | কাস্টম ওয়েব/ডেটাবেজ অ্যাপ্লিকেশন, JSON লগ এবং API-ভিত্তিক ইনজেশন আর্কিটেকচার |

### Phase 04 — Log Engineering and Data Quality (লগ ইঞ্জিনিয়ারিং ও ডেটা কোয়ালিটি)
| ফাইল | বিষয়ের বিবরণ |
|---|---|
| [`12-log-parsing-and-decoders.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/12-log-parsing-and-decoders.md) | কাস্টম লগ ডিকোডার, রেজেক্স (Regex) পার্সিং এবং স্ট্রাকচার্ড ফিল্ড এক্সট্রাকশন |
| [`13-log-normalization-and-timestamps.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/13-log-normalization-and-timestamps.md) | ফিল্ড নরমালাইজেশন স্কিমা, টাইমস্ট্যাম্প ফরম্যাট ও টাইমজোন সিনক্রোনাইজেশন (NTP) |
| [`14-data-ingestion-validation.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/14-data-ingestion-validation.md) | সোর্স থেকে ইনডেক্সার পর্যন্ত এন্ড-টু-এন্ড লগ ভ্যালিডেশন এবং মিসিং ফিল্ড অ্যানালাইসিস |
| [`15-eps-and-performance-testing.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/15-eps-and-performance-testing.md) | থ্রুপুট, ল্যাটেন্সি, ইনজেশন বাফারিং, কিউ ব্যাকলগ ও স্ট্রেস/লোড টেস্টিং |

### Phase 05 — Detection Engineering and Testing (ডিটেকশন ইঞ্জিনিয়ারিং ও টেস্টিং)
| ফাইল | বিষয়ের বিবরণ |
|---|---|
| [`16-detection-rule-engineering.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/16-detection-rule-engineering.md) | ডিটেকশন লজিক ডেভেলপমেন্ট, থ্রেশহোল্ড, সিভিয়ারিটি ও মাল্টিপল ইভেন্ট কোরিলেশন |
| [`17-detection-rule-testing.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/17-detection-rule-testing.md) | পজিটিভ, নেগেটিভ ও রিগ্রেশন টেস্টিং এবং এটাক সিমুলেশন (Atomic Red Team) |
| [`18-false-positive-tuning.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/18-false-positive-tuning.md) | ফলস পজিটিভ টিউনিং, ব্যাসলাইন ডেভিয়েশন এবং অ্যালার্ট ফ্যাটিগ কমানোর কৌশল |
| [`19-rule-version-control-and-cicd.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/19-rule-version-control-and-cicd.md) | Detection-as-Code, Git ভার্সন কন্ট্রোল, CI/CD পাইপলাইন ও অটোমেটেড রোলব্যাক |
| [`20-mitre-attack-coverage.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/20-mitre-attack-coverage.md) | MITRE ATT&CK টেকনিক ম্যাপিং, ভিজ্যুয়ালাইজেশন এবং ডিটেকশন কভারেজ গ্যাপ বিশ্লেষণ |

### Phase 06 — Production Operations & Hardening (প্রোডাকশন অপারেশনস ও স্কেলিং)
| ফাইল | বিষয়ের বিবরণ |
|---|---|
| [`21-dashboard-and-alerting-engineering.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/21-dashboard-and-alerting-engineering.md) | অপারেশনাল ড্যাশবোর্ড ডিজাইন, পাইপলাইন ট্র্যাকিং এবং ইনফ্রাস্ট্রাকচার অ্যালার্টিং |
| [`22-ha-and-failover-design.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/22-ha-and-failover-design.md) | হাই অ্যাভেইলেবিলিটি (HA), ক্লাস্টার ফেইলওভার, মেসেজ ব্রোকার ও লগ ডিউরেবিলিটি |
| [`23-backup-and-disaster-recovery.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/23-backup-and-disaster-recovery.md) | ব্যাকআপ পলিসি, RTO/RPO টার্গেট নির্ধারণ এবং Disaster Recovery (DR) রিকভারি ড্রিল |
| [`24-siem-security-hardening.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/24-siem-security-hardening.md) | RBAC, MFA, TLS মিউচুয়াল অথেনটিকেশন, সিক্রেট ম্যানেজমেন্ট ও হার্ডেনিং |
| [`25-siem-self-monitoring.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/25-siem-self-monitoring.md) | এজেন্ট হেলথ মনিটরিং, হার্টবিট ফেইলিওর, ইনজেশন গ্যাপ ও সার্ভিস ওয়াচডগ |
| [`26-storage-and-retention-management.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/26-storage-and-retention-management.md) | Hot/Warm/Cold স্টোরেজ টায়ারিং, শার্ড ম্যানেজমেন্ট, ইন্ডেক্স লাইফসাইকেল (ILM) ও আর্কাইভ |
| [`27-scaling-and-capacity-growth.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/27-scaling-and-capacity-growth.md) | ৫০০ থেকে ৫,০০০+ এন্ডপয়েন্টে হরাইজন্টাল স্কেলিং ও মাল্টি-সাইট ডিস্ট্রিবিউটেড আর্কিটেকচার |
| [`28-upgrade-and-change-management.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/28-upgrade-and-change-management.md) | জিরো-ডাউনটাইম আপগ্রেড সিকোয়েন্স, ডিপেন্ডেন্সি চেকিং এবং চেঞ্জ ম্যানেজমেন্ট |
| [`29-troubleshooting-scenarios.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/29-troubleshooting-scenarios.md) | বাস্তব প্রোডাকশন বিভ্রাট (ডিস্ক ফুল, পাইপলাইন হ্যাং, ড্রপড প্যাকেট) ও রুট কজ সমাধান |
| [`30-end-to-end-enterprise-project.md`](file:///home/boni/Documents/boniyeamin_resume/interview/siem-engineer-interview-mastery/bangla/30-end-to-end-enterprise-project.md) | সম্পূর্ণ ৫০০ এন্ডপয়েন্ট এন্টারপ্রাইজ প্রজেক্টের ক্যাপস্টোন ডিজাইন ও সাইন-অফ ডক |

---

## 🏗️ প্রতিটি মডিউল ফাইলের স্ট্যান্ডার্ড কাঠামো

প্রতিটি টেকনিক্যাল টপিক যেন প্রফেশনাল মান বজায় রাখে, সেজন্য প্রতিটি ফাইল (`01` থেকে `30`) নিচের কাঠামোর উপর ভিত্তি করে গঠিত:

1. **Learning Objectives** — এই টপিক থেকে নির্দিষ্ট কী কী দক্ষতা অর্জিত হবে।
2. **Real-World Scenario** — এন্টারপ্রাইজ বাস্তব জীবনের একটি জটিল সমস্যা ও প্রেক্ষাপট।
3. **SIEM Engineer Interview Questions** — বেসিক, ইন্টারমিডিয়েট ও অ্যাডভান্সড আর্কিটেকচারাল প্রশ্নমালা (১০টি সাব-টপিক × ৫টি প্রশ্ন = ৫০টি প্রশ্ন)।
4. **Architecture & Design** — কম্পোনেন্ট ডিপেন্ডেন্সি, পোর্ট, কমিউনিকেশন ও ডেটা ফ্লো ডায়াগ্রাম।
5. **Implementation & Configuration** — কনফিগারেশন ফাইল, সিনট্যাক্স ও কমান্ডের বাস্তব উদাহরণ।
6. **Capacity Calculations** — EPS, মেমোরি, আইওপিস (IOPS) ও স্টোরেজ সাইজিংয়ের গানিতিক সূত্র ও হিসাব।
7. **Testing & Validation** — টেস্ট কেস, প্রত্যাশিত ফলাফল এবং ভ্যালিডেশন পদ্ধতি।
8. **Troubleshooting & Remediation** — সম্ভাব্য ফেইলিওর, রুট কজ এবং ট্রাবলশুটিং স্টেপস।
9. **Model Interview Answer** — ইন্টারভিউ বোর্ডে বলার উপযোগী পেশাদার টেকনিক্যাল উত্তর।
10. **Follow-Up Questions** — ইন্টারভিউয়ারের সম্ভাব্য গভীর ফলো-আপ প্রশ্ন ও তার ব্যাখ্যা।

---

## ⚙️ প্র্যাকটিক্যাল ইঞ্জিনিয়ারিং নীতিমালা

* **তথ্য-ভিত্তিক সাইজিং:** ধারণার উপর ভিত্তি করে নয়, মেজার্ড EPS ও কাজের ধরণ অনুযায়ী হার্ডওয়্যার নির্ধারণ করা।
* **এন্ড-টু-এন্ড ভ্যালিডেশন:** এজেন্ট শুধু ‘কানেক্টেড’ দেখালেই দায়িত্ব শেষ নয়; সোর্স থেকে শুরু করে ইনডেক্সারের সার্চ রেজাল্ট পর্যন্ত যাচাই করা।
* **জিরো প্রোডাকশন ক্র্যাশ:** টেস্ট এনভায়রনমেন্ট ও পজিটিভ/নেগেটিভ টেস্টিং ছাড়া কোনো কনফিগারেশন বা রুল প্রোডাকশনে পুশ না করা।
* **রুল ভার্সন কন্ট্রোল:** সমস্ত রুল, ডিকোডার ও পাইপলাইন কনফিগারেশন Git রিপোজিটরিতে রাখা ও চেঞ্জ রিভিউ নিশ্চিত করা।
* **সিকিউরিটি ফার্স্ট:** SIEM যাতে কোনোভাবেই অ্যাটাকারদের টার্গেটে পরিণত না হয়—TLS, RBAC, আইসোলেটেড ম্যানেজমেন্ট নেটওয়ার্ক এবং অডিট ট্রেইল বাধ্যতামূলক রাখা।

---

## 🔄 পড়াশোনা ও অনুশীলনের সাইকেল

প্রতিটি টপিকের ক্ষেত্রে নিচের চক্রটি অনুসরণ করা হবে:

$$\text{Understand} \longrightarrow \text{Design} \longrightarrow \text{Implement} \longrightarrow \text{Test} \longrightarrow \text{Troubleshoot} \longrightarrow \text{Document} \longrightarrow \text{Explain}$$

শুধু থিওরি মুখস্থ করে কোনো টপিক সমাপ্ত বলে গণ্য হবে না; ল্যাবে কনফিগার করে, টেস্টিং ও ফেইলিওর সিনারিও হ্যান্ডেল করার সক্ষমতা অর্জন করাই মূল উদ্দেশ্য।

---

## 🏆 চূড়ান্ত লক্ষ্য

একজন যোগ্য ও আত্মবিশ্বাসী **SIEM Engineer** হিসেবে যেকোনো এন্টারপ্রাইজ পরিবেশে লগ পাইপলাইন ডিজাইন, বাস্তবায়ন, ডিটেকশন টিউনিং এবং সার্বিক ইনফ্রাস্ট্রাকচার মেইনটেইন করার পূর্ণ সক্ষমতা অর্জন করা।

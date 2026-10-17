# 02 — SIEM Architecture Design

## SIEM Engineer Interview Questions

### Topic 01: Enterprise SIEM Architecture

1. একটি প্রতিষ্ঠানে ৫০০টি Windows ও Linux endpoint, Firewall, VPN, Switch এবং Critical Server রয়েছে। সম্পূর্ণ SIEM architecture কীভাবে design করবে?
2. একটি enterprise SIEM-এর প্রধান components কীভাবে সাজাবে এবং প্রতিটি component-এর মধ্যে data flow কীভাবে নির্ধারণ করবে?
3. Centralized, distributed এবং hybrid SIEM architecture-এর মধ্যে কীভাবে সিদ্ধান্ত নেবে?
4. প্রতিষ্ঠানের Head Office, Branch Office, Data Center এবং Cloud Environment থেকে log সংগ্রহের জন্য architecture কীভাবে design করবে?
5. একটি SIEM architecture diagram তৈরি করার সময় কোন কোন component, connection, dependency এবং trust boundary অন্তর্ভুক্ত করবে?

### Topic 02: SIEM Components & Data Flow

6. SIEM Agent, Manager, Collector, Processing Engine, Indexer এবং Dashboard-এর মধ্যে communication architecture কীভাবে design করবে?
7. Endpoint থেকে তৈরি হওয়া একটি event কীভাবে collection, processing, detection, indexing এবং searching-এর মধ্য দিয়ে যাবে, তার end-to-end data flow কীভাবে নির্ধারণ করবে?
8. SIEM Manager এবং Indexer আলাদা server-এ রাখার প্রয়োজন কখন হবে?
9. Log collection ও indexing একই server-এ রাখলে কী ধরনের architectural limitation তৈরি হতে পারে?
10. Dashboard, API, Manager এবং Indexer-এর connectivity ও dependency কীভাবে design করবে?

### Topic 03: Log Collection Architecture

11. ৫০০টি endpoint থেকে সরাসরি SIEM-এ log পাঠানো এবং মাঝখানে centralized collector ব্যবহার করার মধ্যে কীভাবে সিদ্ধান্ত নেবে?
12. বিভিন্ন branch office-এর Firewall ও Switch থেকে log সংগ্রহের জন্য local collector নাকি central collector ব্যবহার করবে?
13. Syslog receiver, agent-based collection এবং API-based integration একই architecture-এ কীভাবে সমন্বয় করবে?
14. Network bandwidth সীমিত হলে remote location থেকে log forwarding কীভাবে design করবে?
15. Collector unavailable হলে log buffering, forwarding retry এবং recovery-এর architecture কীভাবে নির্ধারণ করবে?

### Topic 04: Network Segmentation & Communication

16. SIEM architecture-এ endpoint, management network, collection layer, indexing layer এবং dashboard access-এর জন্য network segmentation কীভাবে করবে?
17. বিভিন্ন VLAN ও subnet থেকে SIEM components-এর মধ্যে কোন communication path প্রয়োজন, তা কীভাবে নির্ধারণ করবে?
18. SIEM deployment-এর জন্য firewall rules ও network ports কীভাবে document করবে?
19. Untrusted network থেকে log গ্রহণ করার সময় secure transport, source validation এবং access restriction কীভাবে design করবে?
20. SIEM management interface-কে সাধারণ endpoint network থেকে আলাদা রাখার architecture কীভাবে তৈরি করবে?

### Topic 05: High Availability & Resilience Design

21. SIEM-এর Manager, Indexer, Collector অথবা Dashboard failure হলে monitoring service সচল রাখার architecture কীভাবে design করবে?
22. একটি single-node SIEM থেকে production-grade redundant architecture-এ কীভাবে migrate করবে?
23. Log collection-এর সময় temporary network outage হলেও data loss-এর ঝুঁকি কমানোর জন্য কী architecture প্রয়োজন?
24. High availability-এর জন্য redundancy, health checks, failover এবং recovery mechanism কীভাবে নির্ধারণ করবে?
25. SIEM-এর একটি component failure হলে অন্য components-এর ওপর কী প্রভাব পড়বে, তা deployment-এর আগে কীভাবে মূল্যায়ন করবে?

### Topic 06: Scalability & Distributed Design

26. ৫০০ endpoint থেকে ৫,০০০ endpoint-এ উন্নীত হলে কোন architectural components scale করতে হবে, তা কীভাবে নির্ধারণ করবে?
27. Log ingestion বাড়লে collection, processing এবং indexing layer আলাদাভাবে scale করার architecture কীভাবে তৈরি করবে?
28. একটি প্রতিষ্ঠানের একাধিক data center থাকলে distributed collection ও centralized search-এর architecture কীভাবে design করবে?
29. Multi-tenant environment-এ বিভিন্ন department বা business unit-এর log ও access কীভাবে পৃথক রাখবে?
30. ভবিষ্যতে নতুন endpoint, cloud services এবং additional log sources যুক্ত করার সুবিধা রেখে architecture কীভাবে design করবে?

### Topic 07: Storage & Data Lifecycle Architecture

31. SIEM architecture-এ ingestion, indexing, searchable storage, archive এবং backup-এর ভূমিকা কীভাবে আলাদা করবে?
32. High-volume log source-এর জন্য storage ও indexing architecture কীভাবে design করবে?
33. ৯০ দিনের searchable retention এবং দীর্ঘমেয়াদি archival requirement থাকলে data lifecycle কীভাবে design করবে?
34. Replica, shard, backup এবং archive-এর মধ্যে পার্থক্য বিবেচনা করে storage architecture কীভাবে নির্ধারণ করবে?
35. Storage failure বা indexing interruption-এর সময় data availability ও recoverability বজায় রাখার architecture কীভাবে তৈরি করবে?

### Topic 08: Security & Trust Boundaries

36. SIEM architecture-এ administrator, agent, collector, API এবং dashboard-এর trust boundaries কীভাবে নির্ধারণ করবে?
37. Agent-to-manager এবং collector-to-indexer communication নিরাপদ করার জন্য কী ধরনের transport security design করবে?
38. SIEM-এর administrative access, service account এবং API access-এর জন্য least-privilege architecture কীভাবে তৈরি করবে?
39. Sensitive log data বিভিন্ন network zone অতিক্রম করলে encryption ও access restrictions কীভাবে নিশ্চিত করবে?
40. SIEM architecture-এ configuration management, audit logging এবং privileged access monitoring কীভাবে অন্তর্ভুক্ত করবে?

### Topic 09: Architecture Validation & Failure Testing

41. Production deployment-এর আগে SIEM architecture-এর data flow এবং component connectivity কীভাবে validate করবে?
42. Collector, Manager, Indexer অথবা Network Link বন্ধ করে architecture-এর resilience কীভাবে test করবে?
43. Expected EPS-এর তুলনায় বেশি log আসলে architecture কীভাবে প্রতিক্রিয়া দেখায়, তা কীভাবে পরীক্ষা করবে?
44. Architecture design-এ single point of failure রয়েছে কি না, তা কীভাবে শনাক্ত করবে?
45. Pilot deployment-এর test results ব্যবহার করে production architecture কীভাবে validate ও উন্নত করবে?

### Topic 10: Architecture Documentation & Design Decisions

46. SIEM architecture design document-এ কোন কোন diagram, network flow, component dependency এবং sizing assumptions অন্তর্ভুক্ত করবে?
47. Architecture Decision Record (ADR) ব্যবহার করে design decisions ও trade-offs কীভাবে document করবে?
48. Security, performance, availability এবং cost-এর মধ্যে conflicting requirements থাকলে কীভাবে architectural trade-off নির্ধারণ করবে?
49. Production architecture-এ পরিবর্তন আনার আগে impact assessment ও rollback plan কীভাবে তৈরি করবে?
50. একটি SIEM architecture production-ready কি না, তা নির্ধারণের জন্য কোন কোন technical acceptance criteria ব্যবহার করবে?

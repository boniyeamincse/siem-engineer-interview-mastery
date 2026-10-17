# 01 — Requirement Assessment

## SIEM Engineer Interview Questions

### Topic 01: Business & Security Requirements

1. একটি প্রতিষ্ঠানে ৫০০টি endpoint, Firewall, VPN, Switch এবং Critical Server রয়েছে। SIEM deployment শুরু করার আগে কীভাবে business ও security requirements সংগ্রহ করবে?
2. প্রতিষ্ঠানের management যদি শুধু বলে “আমাদের একটি SIEM প্রয়োজন”, তাহলে প্রকৃত technical requirements কীভাবে নির্ধারণ করবে?
3. প্রতিষ্ঠানের security objectives, operational requirements এবং compliance requirements কীভাবে SIEM project-এর scope-এ অন্তর্ভুক্ত করবে?
4. SIEM deployment সফল হয়েছে কি না, তা নির্ধারণের জন্য কী কী measurable acceptance criteria নির্ধারণ করবে?
5. প্রতিষ্ঠানের IT, Network, System Administration এবং Security team-এর কাছ থেকে কী কী তথ্য সংগ্রহ করবে?

### Topic 02: Asset Inventory & Endpoint Assessment

6. প্রতিষ্ঠানের ৫০০টি endpoint-এর মধ্যে কতটি Windows workstation, Linux workstation, Windows Server এবং Linux Server রয়েছে, তা কীভাবে inventory করবে?
7. প্রতিষ্ঠানের asset inventory অসম্পূর্ণ হলে SIEM deployment-এর আগে কীভাবে সঠিক asset list তৈরি করবে?
8. Domain-joined endpoint, standalone system, remote workstation, virtual machine এবং cloud-hosted server-এর জন্য কী ধরনের onboarding requirements নির্ধারণ করবে?
9. Critical server, domain controller, database server এবং সাধারণ workstation-এর logging requirements কীভাবে আলাদা করবে?
10. Asset inventory-তে থাকা endpoint-এর সঙ্গে SIEM-এ onboard করা endpoint-এর তালিকা কীভাবে reconcile করবে?

### Topic 03: Log Source Identification

11. একটি প্রতিষ্ঠানের Windows, Linux, Active Directory, DNS, DHCP, Firewall, VPN, Switch, IDS/IPS এবং Application থেকে কোন কোন log সংগ্রহ করা প্রয়োজন, তা কীভাবে নির্ধারণ করবে?
12. প্রতিষ্ঠানের কোনো গুরুত্বপূর্ণ application-এর log source সম্পর্কে documentation না থাকলে কীভাবে তার logging requirements আবিষ্কার করবে?
13. কোন log source থেকে agent-based collection, কোনটি থেকে Syslog এবং কোনটি থেকে API-based collection প্রয়োজন হবে, তা কীভাবে নির্ধারণ করবে?
14. একটি প্রতিষ্ঠানের সব log source SIEM-এ যুক্ত করা সম্ভব না হলে কোন source-গুলোকে অগ্রাধিকার দেবে?
15. প্রতিটি log source-এর event type, expected volume, collection method, transport protocol এবং ownership কীভাবে document করবে?

### Topic 04: Critical Assets & Business Prioritization

16. প্রতিষ্ঠানের ৫০০টি endpoint-এর মধ্যে কোন asset-গুলো business-critical, তা কীভাবে শনাক্ত করবে?
17. Domain Controller, Active Directory, VPN Gateway, Firewall, Database Server এবং সাধারণ workstation-এর logging priority কীভাবে নির্ধারণ করবে?
18. Business-critical asset-এর জন্য অতিরিক্ত audit logging বা enhanced monitoring প্রয়োজন কি না, তা কীভাবে মূল্যায়ন করবে?
19. কোনো critical server থেকে প্রয়োজনীয় log সংগ্রহ করা সম্ভব না হলে কীভাবে requirement gap document করবে?
20. Asset criticality, business impact এবং logging priority-এর মধ্যে সম্পর্ক কীভাবে নির্ধারণ করবে?

### Topic 05: Log Volume & EPS Requirements

21. SIEM deployment-এর আগে প্রতিটি log source-এর expected EPS এবং daily log volume কীভাবে নির্ধারণ করবে?
22. কোনো প্রতিষ্ঠানে বর্তমান log volume-এর measurement না থাকলে infrastructure sizing-এর জন্য প্রাথমিক volume estimate কীভাবে তৈরি করবে?
23. Normal business hours, peak activity এবং unusual event bursts-এর মধ্যে log volume-এর পার্থক্য কীভাবে মূল্যায়ন করবে?
24. Endpoint সংখ্যা একই থাকলেও log volume কয়েক গুণ বেড়ে গেলে তার কারণ ও infrastructure impact কীভাবে মূল্যায়ন করবে?
25. Requirement assessment-এর সময় কীভাবে average EPS, peak EPS, GB/day এবং ভবিষ্যৎ growth assumptions document করবে?

### Topic 06: Retention & Compliance Requirements

26. প্রতিষ্ঠানের policy অনুযায়ী ৩০ দিন, ৯০ দিন, ১৮০ দিন অথবা এক বছর log retention প্রয়োজন হলে সেই requirement কীভাবে সংগ্রহ করবে?
27. Online searchable retention এবং long-term archive retention-এর requirement কীভাবে আলাদা করবে?
28. Legal, regulatory, audit এবং internal security policy অনুযায়ী কোন log কতদিন সংরক্ষণ করতে হবে, তা কীভাবে নির্ধারণ করবে?
29. Log retention-এর পাশাপাশি log integrity, access control, archival এবং deletion requirements কীভাবে সংগ্রহ করবে?
30. Storage budget সীমিত হলেও compliance অনুযায়ী retention বজায় রাখতে হলে কীভাবে requirement prioritization করবে?

### Topic 07: Infrastructure & Network Requirements

31. SIEM deployment-এর জন্য প্রয়োজনীয় server, virtualization, storage, network এবং operating system requirements কীভাবে সংগ্রহ করবে?
32. বিভিন্ন VLAN ও subnet থেকে SIEM-এ log পাঠানোর জন্য কোন network routes, firewall rules এবং communication ports প্রয়োজন হবে, তা কীভাবে শনাক্ত করবে?
33. SIEM server on-premises, cloud অথবা hybrid environment-এ deploy করার requirement কীভাবে মূল্যায়ন করবে?
34. প্রতিষ্ঠানের network bandwidth সীমিত হলে remote endpoint এবং network device থেকে log collection-এর requirements কীভাবে নির্ধারণ করবে?
35. SIEM deployment-এর আগে DNS, NTP, certificate, proxy এবং network connectivity dependency কীভাবে যাচাই করবে?

### Topic 08: Availability, Backup & Recovery Requirements

36. প্রতিষ্ঠানের SIEM কতক্ষণ unavailable থাকতে পারবে, তা কীভাবে নির্ধারণ করবে?
37. SIEM service failure-এর ক্ষেত্রে business requirement অনুযায়ী RTO এবং RPO কীভাবে নির্ধারণ করবে?
38. Log collection, processing, indexing এবং dashboard availability-এর জন্য আলাদা availability requirements কীভাবে সংগ্রহ করবে?
39. Disaster recovery, backup location, recovery testing এবং configuration restoration-এর প্রয়োজনীয়তা কীভাবে নির্ধারণ করবে?
40. SIEM outage চলাকালে log buffering, queueing এবং recovery-এর পরে backlog processing-এর requirements কীভাবে নির্ধারণ করবে?

### Topic 09: Security, Access & Integration Requirements

41. SIEM administrator, security engineer, auditor এবং read-only user-এর জন্য কী ধরনের access requirements নির্ধারণ করবে?
42. SIEM-এর সঙ্গে Active Directory, ticketing platform, email, notification service এবং অন্যান্য security platform integrate করার requirements কীভাবে সংগ্রহ করবে?
43. API integration-এর জন্য authentication, authorization, credential storage এবং network access-এর requirements কীভাবে নির্ধারণ করবে?
44. Sensitive log data-এর জন্য encryption, access restrictions, audit logging এবং data protection requirements কীভাবে নির্ধারণ করবে?
45. SIEM deployment-এর আগে কোন কোন security dependency, external integration এবং third-party requirement document করা প্রয়োজন?

### Topic 10: Project Scope, Constraints & Acceptance

46. প্রতিষ্ঠানের budget, deployment timeline, available personnel এবং licensing constraints কীভাবে SIEM project design-এ বিবেচনা করবে?
47. Pilot deployment-এর জন্য কোন endpoint ও log source নির্বাচন করবে এবং pilot সফল হওয়ার acceptance criteria কীভাবে নির্ধারণ করবে?
48. Requirement assessment-এর সময় কোন assumptions, risks, technical dependencies এবং unresolved questions document করবে?
49. SIEM project-এর requirement পরিবর্তিত হলে scope, capacity estimate এবং implementation plan কীভাবে পুনর্মূল্যায়ন করবে?
50. Requirement assessment সম্পন্ন হওয়ার পরে architecture design ও capacity planning শুরু করার জন্য কোন কোন document, inventory এবং measurement প্রস্তুত থাকতে হবে?


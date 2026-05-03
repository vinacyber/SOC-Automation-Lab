# Building a SOC Detection Lab: Suricata & Wazuh

## Introduction
In this project, I built a Security Operations Center (SOC) home lab to learn how network attacks are detected and managed. I used **Suricata** as my Network Intrusion Detection System (NIDS) and **Wazuh** as my Security Information and Event Management (SIEM) platform.

## Lab Architecture
- **Attacker**: Kali Linux (Generating Nmap scans and Pings)
- **Target/IDS**: Ubuntu Server (Running Suricata to sniff traffic)
- **SIEM**: Wazuh (Collecting and visualizing alerts)
  
![Kali Attacker Terminal](Kali%20(attacker).png)

## Key Achievements & Troubleshooting

### 1. Custom Detection Engineering
I moved away from generic alerts by writing specific Suricata signatures.
- **Problem**: Broad rules caused "Rule Shadowing," where specific attacks were mislabeled as general traffic.
- **Solution**: Implemented `flags:S` to isolate SYN Stealth scans and used `priority` levels to ensure critical alerts fire first.

### 2. SIEM Log Enrichment
I optimized how Wazuh displays data to improve analyst response time.
- **Problem**: Wazuh displayed a generic "Rule 86601" for all Suricata alerts.
- **Solution**: Created a custom XML override in `local_rules.xml` to map the `alert.signature` field directly to the dashboard description.

## Results
- **Successful Detection**: ICMP Pings and Nmap Stealth Scans are now identified by their specific names in the dashboard.
- **Improved Visibility**: High-noise aggressive scans (-A) are filtered through thresholds to prevent dashboard fatigue.

![Nmap Stealth Scan Alert](Nmap%20Syn%20scan.png)
![ICMP Ping Alert](ping%20alert.png)
![Aggressive Scan Noise](Aggressive%20alert.png)

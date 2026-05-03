# Building a SOC Detection Lab: Suricata & Wazuh

## Introduction
In this project, I built a Security Operations Center (SOC) home lab to learn how network attacks are detected and managed. I used **Suricata** as my Network Intrusion Detection System (NIDS) and **Wazuh** as my Security Information and Event Management (SIEM) platform.

## Lab Architecture
- **Attacker**: Kali Linux (Generating Nmap scans and Pings)
- **Target/IDS**: Ubuntu Server (Running Suricata to sniff traffic)
- **SIEM**: Wazuh (Collecting and visualizing alerts)

graph LR
    subgraph Attacker_Zone [Attacker]
    A[Kali Linux<br/>192.168.244.129]
    end

    subgraph Target_Zone [Victim Network]
    W[Windows 10<br/>192.168.244.131]
    U[Ubuntu Server<br/>192.168.244.134]
    end

    subgraph Monitoring_Zone [SOC Management]
    S{Suricata NIDS}
    Z[Wazuh SIEM]
    end

    A -- "Nmap / Ping Attack" --> W
    W -- "Traffic Captured" --> U
    U -- "Rules Processing" --> S
    S -- "JSON Alert Generated" --> Z
    Z -- "Visualizes" --> D[Analyst Dashboard]

    style A fill:#ff9999,stroke:#333,stroke-width:2px
    style W fill:#fff,stroke:#333
    style U fill:#fff,stroke:#333
    style S fill:#ffff99,stroke:#333
    style Z fill:#99ccff,stroke:#333,stroke-width:2px

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

### 1. Attack Simulation
Captured the initial reconnaissance phase from the Attacker's perspective.
![Kali Attacker Terminal](Kali%20(attacker).png)

### 2. Detection & SIEM Visibility
Successfully identified and categorized the traffic within the Wazuh dashboard.
![Nmap Stealth Scan Alert](Nmap%20Syn%20scan.png)
![ICMP Ping Alert](ping%20alert.png)

## 🧠 Skills Developed
- **Network Traffic Analysis**: Interpreting TCP/IP headers and protocol behavior.
- **Rule Development**: Writing and tuning IDS signatures in Suricata.
- **SIEM Configuration**: Engineering custom decoders and rules for Wazuh.
- **Vulnerability Assessment**: Utilizing Nmap for reconnaissance and service identification.
- **Troubleshooting**: Diagnosing service errors and log-flow discrepancies in a Linux environment.

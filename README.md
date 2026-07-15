# Elastic SIEM Home Lab

**Fleet Endpoint Deployment · Sysmon & Auditd Monitoring · Brute-Force Attack Simulation · Detection Engineering**

A hands-on Security Operations lab built on the Elastic Stack (Elasticsearch, Kibana, Fleet Server, Elastic Agent), covering multi-platform endpoint onboarding, real attack simulation, and validated threat detection — with every infrastructure failure documented as honestly as every success.

---

## 📌 Overview

This project is not a single-screenshot "I ran an attack tool" demo. It's an end-to-end build: deploy a SIEM manager, enroll real endpoints across two operating systems, configure host-level monitoring, simulate credential attacks, and validate the resulting detections directly against live data — while documenting the real infrastructure problems (certificate trust, DHCP changes, credential drift, misrouted log output) encountered and fixed along the way.

**Core focus areas:**
- SOC-style detection engineering and threat hunting
- Log analysis and normalization (ECS) across Sysmon, auditd, and sshd sources
- Networking fundamentals — subnetting, DHCP, TCP/IP, TLS trust
- Endpoint and infrastructure device management (Fleet Server, agents, host firewalls)
- Honest documentation of environmental limitations (Windows Home RDP restriction)
- AI-assisted troubleshooting, query drafting, and documentation

---

## 🏗️ Lab Architecture

| Component | Role |
|---|---|
| **elk-server** (Ubuntu 22.04, VirtualBox VM) | Elasticsearch + Kibana + Fleet Server — central SIEM manager |
| **linux-agent** (Ubuntu 22.04, VirtualBox VM) | Monitored Linux endpoint — Elastic Agent + auditd (file integrity monitoring on `/etc/passwd`, `/etc/shadow`) |
| **windows-agent "BATMAN"** (Windows 10 Home, physical machine) | Monitored Windows endpoint — Elastic Agent + Sysmon Operational channel |
| **attacker** (Kali Linux, VirtualBox VM) | Runs Hydra for SSH/RDP brute-force simulation |

All machines share a private subnet so agent-to-Fleet-Server and attacker-to-endpoint traffic behaves exactly as it would in production.

---

## 🔧 What Was Built

- **SIEM deployment**: Elasticsearch, Kibana, and Fleet Server installed and hardened on a dedicated Ubuntu host
- **Multi-platform agent enrollment**: Elastic Agent deployed and Fleet-managed on both a Linux VM and a real Windows machine
- **Endpoint monitoring**: Sysmon Operational channel on Windows; auditd watch rules on Linux
- **Attack simulation**: SSH brute-force via Hydra (MITRE ATT&CK **T1110**), plus an attempted RDP brute-force
- **Detection validation**: every use case confirmed with ECS-normalized queries in Kibana Discover, re-verified over both short and long time windows
- **Infrastructure troubleshooting**: TLS certificate trust failures, DHCP-driven IP changes, credential rotation drift, and Fleet output misconfiguration — all diagnosed and resolved

---

## 🎯 Detections Implemented

| Use Case | Query | MITRE ATT&CK |
|---|---|---|
| SSH brute-force | `event.outcome: "failure" and process.name: "sshd"` | T1110.001 – Password Guessing |
| File integrity (auditd) | `auditd.log.key: "passwd_changes"` | T1098 / T1136 – Persistence |
| Sysmon network visibility | `winlog.channel: "Microsoft-Windows-Sysmon/Operational" and event.code: "3"` | T1046 – Discovery |

---

## ⚠️ Known Limitation

RDP brute-force detection could not be completed as originally scoped: the physical Windows endpoint runs **Windows 10 Home**, which does not support hosting inbound RDP sessions at the OS level — no firewall or registry change can work around this. The Sysmon pipeline itself was independently validated using a generic connection probe instead. Documented, not hidden.

---

## 🧠 Skills Demonstrated

- **SOC operations** — endpoint onboarding, health triage, alert validation
- **Networking** — subnetting, DHCP behavior, TCP/IP, TLS/PKI trust issues
- **Devices & infrastructure** — Fleet Server (control plane), endpoint agents (sensors), host-based firewalls
- **SIEM** — Elastic Stack deployment, ECS log normalization, Kibana Discover
- **Threat hunting** — hypothesis-driven querying rather than default-dashboard reliance
- **Log analysis** — correlating Sysmon, auditd, and authentication logs to separate signal from noise
- **AI-assisted workflow** — used for real-time troubleshooting triage, detection query drafting, and converting raw session logs into structured documentation

**Not yet covered in this lab** (noted honestly rather than implied): SOAR/automated response playbooks, dedicated router/switch/firewall hardware configuration, phishing analysis, malware analysis. Planned as next steps.

---

## 📄 Documentation

Full write-up with architecture diagrams, step-by-step build log, screenshots, and MITRE ATT&CK mapping is included in this repo — see `/docs`.

---

## 📬 Contact

- LinkedIn: [pavankumar022](https://linkedin.com/in/pavankumar022)
- GitHub: [pavankumar022](https://github.com/pavankumar022)
- Email: pavankumar797524@gmail.com

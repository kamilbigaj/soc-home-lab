# SOC Home Lab

## Project Overview
The goal of this project is to build and maintain a hands-on Security Operations Center (SOC) home lab environment. It serves as a practical testing ground for log collection, threat detection, telemetry analysis, and incident response exercises.

---

## Environment & Architecture

- **Hardware / Infrastructure:** MacBook Pro (Apple Silicon M3 Pro, 18GB RAM) running VMware Fusion
- **Operating Systems & Endpoints:**
  - **Windows 11 (ARM):** Endpoint log source (Windows Event Logs, Sysmon)
  - **Linux (Ubuntu):** Telemetry & activity logs (`syslog`, `auth.log`)
  - **SIEM Platform:** [Planned] (e.g., Wazuh / Splunk / Elastic Stack)

---

## Current Status & Roadmap

### Progress Checklist
- [x] Repository and documentation initialized
- [x] VMware Fusion installed and configured
- [x] Windows 11 endpoint / VM configured
- [ ] Linux endpoint / VM configured
- [ ] Centralized logging / log shipper configuration
- [ ] SIEM deployment and integration
- [ ] Baseline telemetry and noise filtering
- [ ] Simulated attack scenarios and incident analysis

---

## Next Steps
1. Finalize VM setup for Linux (Ubuntu) target.
2. Configure audit policies and log ingestion (Windows Security Logs & Linux `auth.log`).
3. Deploy a lightweight SIEM (e.g., Wazuh) to monitor events.
4. Analyze at least 4 common security event scenarios (e.g., failed brute-force logins, privilege escalation).

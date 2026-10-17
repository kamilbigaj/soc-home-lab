# SOC Home Lab

A hands-on learning project for practicing security log analysis across Windows and Linux virtual machines. The current phase focuses on generating controlled events, identifying relevant fields, documenting observations, and recognizing the limits of individual log records.

> **Project status:** Initial lab setup and four event investigations documented. Log analysis is currently performed locally; centralized SIEM collection and automated detection are future improvements.

## Objectives

- Practice navigating and filtering Windows Event Viewer logs.
- Use `journalctl` to inspect Linux authentication and service logs.
- Document authentication activity, privileged command execution, and service lifecycle events.
- Explain what each event supports, what it does not prove, and what additional evidence an analyst would need.
- Build a foundation for future SIEM ingestion, detection engineering, and incident-response exercises.

## Lab Environment

| Component | Configuration |
|---|---|
| Windows guest | Windows 11 Pro |
| Linux guest | Ubuntu |
| Virtualization | VMware-based virtual machines (exact product/version not recorded) |
| Guest networking | NAT, as recorded during lab verification |
| Windows log source | Event Viewer → Windows Logs → Security |
| Linux log sources | `journalctl`, including `sudo` and `cron` service messages |
| Analysis method | Local, manual review |

The guest machines use NAT networking. NAT commonly allows outbound connectivity while preventing unsolicited inbound connections by default, but it is **not the same as complete isolation**. Port forwarding, host configuration, and firewall rules can affect exposure. Confirm that no unnecessary port-forwarding rules are configured before describing the lab as not externally reachable.

## Investigations Documented

### Windows

- **Event ID 4625 — Failed logon:** A deliberately incorrect password was entered during a controlled test. The recorded event is timestamped `2026-10-17 11:05:21`.
- **Event ID 4624 — Successful logon:** A successful logon followed at `2026-10-17 11:05:25`, four seconds after the failed-logon event.

The sequence is consistent with the controlled test described in the notes. The event records alone do not establish a user's identity or intent. Some fields, including the account name and the successful event's actual Logon Type, were not preserved in the supplied notes and should be completed from the original event details if they are still available.

### Linux

- **`sudo` authentication and command execution:** An authentication failure was observed at approximately `11:09:50`, followed by a `sudo` record at `11:09:54` for `/usr/bin/ls /root` executed as `root`.
- **`cron` service restart:** Service messages recorded the stop/start sequence at approximately `11:14:35–11:14:36`, with the restarted process reported as PID `5626`.

The `sudo` authentication event at `11:09` is separate from the `cron` restart at `11:14`. To attribute the restart to a user, inspect `sudo` records around `11:14:35`; do not treat the earlier event as proof of who initiated the restart.

See the event notes for the detailed observations and limitations:

- [Windows event analysis](notes/windows-events.md)
- [Linux event analysis](notes/linux-events.md)
- [Lab architecture](architecture.md)
- [Week 1 summary](WEEK1-SUMMARY.md)

## Current Capabilities

- Manual Windows Security log filtering by Event ID.
- Basic Linux journal filtering by command and service.
- Documentation of event context, key fields, and evidence limitations.
- Initial comparison of Windows event records with Linux journal messages.

## Planned Improvements

- Add redacted screenshots of the original evidence.
- Confirm missing Windows event fields from the original records.
- Correlate the `cron` restart with the `sudo` journal entries from the same time window.
- Deploy a SIEM such as Wazuh and forward logs from both guest systems.
- Build and test detection logic using controlled scenarios.
- Write short incident-triage notes and response playbooks based on observed data.

## Repository Structure

```text
soc-home-lab/
├── README.md
├── architecture.md
├── WEEK1-SUMMARY.md
├── notes/
│   ├── windows-events.md
│   └── linux-events.md
└── screenshots/
    └── README.md
```

## Privacy and Safety

This is an educational lab using controlled actions on virtual machines. Before publishing screenshots, review them for usernames, hostnames, email addresses, private/public IP addresses, tokens, and unrelated desktop content. The written notes in this repository use generalized identifiers where practical; screenshots should be redacted to match. Never publish passwords, tokens, private keys, or other credentials.

## Scope and Limitations

This project currently demonstrates manual log review, not a production SOC deployment. It does not yet include centralized collection, automated alerting, tested detection rules, or a validated incident-response workflow. Findings are limited to the event data recorded in the notes.

**Maintainer:** [kamilbigaj](https://github.com/kamilbigaj)  
**Last updated:** 2026-10-17

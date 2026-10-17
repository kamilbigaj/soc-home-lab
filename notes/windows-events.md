# Windows Event Analysis

This file documents two Windows Security events generated or observed during a controlled test on a Windows 11 Pro virtual machine. Host identifiers are generalized in this report for privacy; consult the original event records for the exact values.

## Event 1 — Failed Logon (Event ID 4625)

**Observed date:** 2026-10-17  
**Observed timestamp:** 2026-10-17 11:05:21 (local system time; timezone not captured in the supplied notes)

### Action performed

The Windows session was locked with `Win + L`, and an incorrect password was deliberately entered to generate a failed-logon event.

### Log location

- **System:** Windows 11 Pro
- **Path:** Event Viewer → Windows Logs → Security
- **Provider:** `Microsoft-Windows-Security-Auditing`
- **Event ID:** `4625`
- **Logon Type:** `2` (as recorded in the supplied notes)

### Relevant fields

| Field | Value |
|---|---|
| Timestamp | `2026-10-17 11:05:21` |
| Event ID | `4625` |
| Host | `LAB-WIN11` (generalized identifier) |
| Logon Type | `2` |
| Account name | Not preserved in the supplied notes; retrieve from **Account For Which Logon Failed** if the original event is available |
| Failure reason | Not preserved in the supplied notes; retrieve the actual **Failure Reason**, **Status**, and **Sub Status** values from the event |

### Analyst interpretation

The event records a failed logon attempt on the Windows guest at the time shown above. Logon Type 2 represents an interactive logon. In the context of the controlled test described here, the event is consistent with the deliberately incorrect password entry.

A single failed-logon event is not enough to distinguish an ordinary user mistake from suspicious activity. Review surrounding events, the account targeted, the failure status, and any repeated attempts before drawing a conclusion.

### What this event does not establish

- It does not establish whether the failed attempt was accidental or malicious without supporting context.
- It does not reveal the password that was entered.
- It does not, by itself, identify the person who was at the keyboard.
- The account name and failure details are missing from the supplied notes and must not be guessed.

### Recommended follow-up

1. If available, reopen the original 4625 record and fill in the account name and the exact failure fields.
2. Review nearby 4624 and 4625 events to understand the sequence.
3. Compare the activity with the controlled test notes before interpreting it as suspicious.

---

## Event 2 — Successful Logon (Event ID 4624)

**Observed date:** 2026-10-17  
**Observed timestamp:** 2026-10-17 11:05:25 (local system time; timezone not captured in the supplied notes)

### Action performed

The correct password was entered after the earlier failed attempt, and access to the Windows session was restored.

### Log location

- **System:** Windows 11 Pro
- **Path:** Event Viewer → Windows Logs → Security
- **Provider:** `Microsoft-Windows-Security-Auditing`
- **Event ID:** `4624`

### Relevant fields

| Field | Value |
|---|---|
| Timestamp | `2026-10-17 11:05:25` |
| Event ID | `4624` |
| Host | `LAB-WIN11` (generalized identifier) |
| Account name | Not preserved in the supplied notes; retrieve it from the event's **New Logon** section if available |
| Logon Type | Not confirmed in the supplied notes; copy the exact value from the original event rather than guessing between `2` and `7` |

### Analyst interpretation

The event records a successful logon at 11:05:25, four seconds after the 4625 event documented above. Given the controlled-test description, this sequence is consistent with an incorrect password being followed by a successful authentication.

In general, a successful logon is not inherently benign or malicious. Its significance depends on context, including the account, logon type, source information when applicable, the surrounding authentication events, and activity after the logon.

### What this event does not establish

- It does not, by itself, prove who used the account or whether the access was authorized.
- It does not prove that the preceding failed event and the successful event had the same human actor, even when they occur close together.
- The account name and actual Logon Type are not preserved in the supplied notes.

### Correlation note

The two supplied timestamps are four seconds apart. In this controlled test, that sequence matches the described action. In a real investigation, an analyst would verify the account and logon details and review the relevant surrounding events before reaching a conclusion.

---

**Windows events documented:** 2  
**Last updated:** 2026-10-17


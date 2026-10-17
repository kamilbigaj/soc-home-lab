# Linux Event Analysis

This file documents two types of activity observed on an Ubuntu virtual machine: a `sudo` authentication/command record and a `cron` service restart. The host and user identifiers below are generalized for privacy.

## Event 3 — `sudo` Authentication and Command Execution

**Observed date:** 2026-10-17  
**Relevant timestamps:** approximately 11:09:50 (authentication failure) and 11:09:54 (command record)

### Action performed

The command `sudo ls /root` was run in a terminal. The first password attempt failed; a subsequent attempt succeeded and the command was run with root privileges.

### Log location

- **System:** Ubuntu
- **Primary source:** `journalctl`
- **Filter used:** `journalctl _COMM=sudo`
- **Alternative source, if configured:** `/var/log/auth.log`
- **Event ID:** Not applicable; this Linux journal record does not use a Windows Event ID.

### Relevant fields

| Field | Value |
|---|---|
| Timestamp | `2026-10-17 11:09:54` for the command record; authentication failure at approximately `11:09:50` |
| Host | `LAB-UBUNTU` (generalized identifier; original host identifier was `kamil-VMware20-1`) |
| Service/process | `sudo` |
| Invoking account | `analyst` (generalized identifier; original account was `kamil`) |
| TTY | `/dev/pts/0` |
| Working directory | `/home/analyst` (generalized path; original path was `/home/kamil`) |
| Target user | `root` |
| Command | `/usr/bin/ls /root` |

### Analyst interpretation

The command record indicates that the invoking account used `sudo` to run `/usr/bin/ls /root` as `root`. The nearby authentication-failure message at approximately 11:09:50 is consistent with the first unsuccessful password entry described in the test notes.

This is a useful example of why authentication outcomes and privileged command records matter in security monitoring. The presence of a `sudo` record alone does not establish malicious activity; the account's expected duties and surrounding activity must be considered.

### What these records do not establish

- They do not show the exact output displayed by `ls`.
- They do not establish the user's intent.
- They do not prove that the activity was unauthorized merely because elevated privileges were used.
- The interpretation of the failed attempt is based on the controlled-test description and timing, not on the authentication-failure line alone.

### Recommended follow-up

Review other authentication and process records around the same timestamp and confirm whether the command was expected for the account. In a production investigation, review the applicable privilege policy and the activity that followed.

---

## Event 4 — `cron` Service Restart

**Observed date:** 2026-10-17  
**Observed timestamps:** approximately 11:14:35–11:14:36

### Action performed

The command `sudo systemctl restart cron` was run to restart the system's cron scheduling service as a controlled lab exercise.

### Log location

- **System:** Ubuntu
- **Source:** systemd journal
- **Filter used:** `journalctl -u cron`
- **Event ID:** Not applicable.

### Relevant fields

| Field | Value |
|---|---|
| Timestamp | `2026-10-17 11:14:35–11:14:36` |
| Host | `LAB-UBUNTU` (generalized identifier) |
| Service | `cron` |
| Managing process | `systemd[1]` |
| Service process | `cron[5626]` (PID as recorded in the supplied notes) |
| Messages | Stop/deactivation and subsequent start of `cron.service` |

### Analyst interpretation

The journal records the `cron` service stopping and starting again. The messages show a service lifecycle change, and the supplied notes identify PID `5626` for the restarted service process.

Service restarts can be routine administrative activity. They are worth investigating when unexpected, especially when the service is security-sensitive or when the timing coincides with other suspicious actions. The service log alone does not identify who requested the restart or why.

### What this record does not establish

- It does not identify the user who initiated the restart.
- It does not explain the reason for the restart.
- It does not show whether a cron job or service configuration was changed before the restart.
- It does not establish that the restart was malicious.

### Correlation required

The earlier `sudo` event documented in Event 3 occurred around 11:09:50–11:09:54, approximately five minutes before this restart. **That earlier event cannot, by itself, establish who initiated the restart at 11:14.** To investigate attribution, inspect `sudo` records from the restart's own time window, for example:

```bash
journalctl _COMM=sudo --since "2026-10-17 11:14:00" --until "2026-10-17 11:15:00"
```

If a matching `sudo systemctl restart cron` record is present, document its actual timestamp, invoking account, and command. If no matching record is found, state that attribution remains unconfirmed.

---

**Linux event types documented:** 2  
**Last updated:** 2026-10-17


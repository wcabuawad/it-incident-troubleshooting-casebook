# Case 001 — Intermittent domain sign-in failures

**Type:** Fictional portfolio scenario  
**Environment:** Windows domain with client workstations and centralized authentication  
**Severity:** Medium (multiple users affected intermittently)  
**Status:** Investigation plan / expected diagnostic path

## 1. User report

Several employees report that they intermittently cannot sign in to their Windows workstations using their domain accounts. Some users succeed after waiting and trying again. A local administrator can sign in to one affected workstation. No confirmed recent domain-wide change has been reported.

## 2. Confirmed facts vs. unknowns

### Confirmed in this fictional scenario
- More than one user has reported the symptom.
- The failure is intermittent.
- At least one affected workstation accepts a local account.

### Not yet known
- Whether affected users are on the same subnet or site.
- Whether the issue occurs on wired and wireless networks.
- Whether domain controllers are reachable from affected clients.
- Whether DNS client settings point to the organization's approved DNS servers.
- Whether event logs show authentication, secure-channel, or name-resolution errors.
- Whether the issue began after a network, DNS, patching, or identity change.

## 3. Initial triage questions

1. What exact error message appears, and at what stage of sign-in?
2. How many users and workstations are affected?
3. Did the issue start at a known time?
4. Are affected devices connected to the corporate network or VPN?
5. Can an unaffected workstation authenticate normally?
6. Are there recent changes to DNS, DHCP, network routing, domain controllers, or endpoint configuration?

Do not ask users to send passwords, MFA codes, or screenshots containing sensitive information.

## 4. Diagnostic hypotheses

| Hypothesis | Evidence to collect | What would support it |
|---|---|---|
| Client DNS misconfiguration | `ipconfig /all`; DNS resolution of domain records | Client uses an unapproved DNS resolver or cannot resolve domain records |
| Network path issue | Gateway reachability; connectivity to approved domain services | Affected subnet cannot reach required services |
| Domain controller availability | Approved monitoring and relevant event logs | Authentication failures correlate with an unavailable or unreachable controller |
| Time synchronization issue | Client time and domain time source | Significant time difference or time-service errors |
| Device secure-channel issue | Relevant system logs and approved domain tools | Evidence isolated to a specific computer account/channel |

These are hypotheses, not conclusions.

## 5. Low-risk diagnostic sequence

Perform checks with authorized access and follow the organization's change-control process.

### Step A — Establish scope
- Record affected users, device names, locations/subnets, timestamps, and exact error text.
- Compare one affected device with one unaffected device.
- Determine whether failures correlate with a specific network segment or time window.

### Step B — Check client network configuration
On an affected Windows workstation, an authorized technician may run:

```powershell
ipconfig /all
ipconfig /flushdns
```

Use `ipconfig /flushdns` only if appropriate for the investigation; capture the original DNS configuration first. The first command is read-only. Flushing the local resolver cache is a limited change and should be documented.

### Step C — Test name resolution
Use the organization's actual domain name in place of `example.internal`:

```powershell
nslookup example.internal
nslookup -type=SRV _ldap._tcp.dc._msdcs.example.internal
```

Expected result: the configured corporate DNS service resolves the domain and relevant service records to valid domain-controller targets.

If resolution fails, investigate client DNS settings and approved DNS infrastructure before changing any settings.

### Step D — Check time and network path
```powershell
w32tm /query /status
Test-NetConnection <approved-domain-controller-hostname> -Port 389
```

Replace the placeholder with an approved hostname. Port testing is only one data point; it does not prove that all authentication dependencies are healthy.

### Step E — Review relevant logs
Review Windows System and security-related logs using approved access and retention procedures. Correlate event timestamps with the reported failures. Do not export or share sensitive log data without authorization.

## 6. Recommended next action by evidence

- **DNS records fail to resolve:** verify the workstation's approved DNS configuration and escalate to the DNS/network owner. Do not substitute public DNS on a domain-joined workstation as a quick fix.
- **Only one workstation is affected:** compare its network/DNS/time configuration with a healthy peer, then investigate device-specific evidence.
- **Multiple devices on one subnet are affected:** prioritize subnet, DHCP, routing, and DNS path investigation.
- **Domain controller service availability is implicated:** escalate to the identity/infrastructure owner with timestamps and collected evidence.
- **Time synchronization is implicated:** verify the configured time hierarchy and follow approved remediation procedures.

## 7. Resolution and verification criteria

Do not close the incident merely because one user can sign in once. Confirm:

- affected users can authenticate consistently;
- the issue no longer reproduces across the affected scope;
- name resolution and relevant service discovery work as expected;
- event logs show no continuing related failures;
- the root cause and any approved changes are documented;
- users and the incident owner receive a concise closure update.

## 8. Operational safety

Never request or record user passwords or MFA codes. Avoid account deletion, domain rejoin, broad DNS changes, or domain-controller restarts as first-line actions. These actions can cause disruption and require evidence, authorization, and a rollback plan.

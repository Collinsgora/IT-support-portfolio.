# Entra ID Device Enrollment Failure — Root Cause Investigation

## Overview

Investigation of an Intune enrollment failure on a domain-joined, Azure AD-joined device with a valid TPM certificate. Despite a healthy local join state, `DeviceAuthStatus` reported `FAILED`, blocking enrollment. Includes a same-day parallel incident: a driver-update rollout that broke printer connectivity across multiple machines.

> Device names, IDs, and thumbprints below are illustrative placeholders. The diagnostic steps and findings reflect a real investigation.

## Ticket Context
- **Issue reported:** "Error can't get there" when attempting to register/enroll a device into Intune.
- **Scope requested:** Check device state across Intune, Entra ID, Azure, and Windows Defender; confirm encryption status.

## Diagnostics Performed

Ran on the affected machine:
```powershell
dsregcmd /status
```

**Sample output (sanitized):**
```
AzureAdJoined      : YES
EnterpriseJoined   : NO
DomainJoined       : YES
DomainName         : CORP
Device Name        : DEVICE-01.corp.local
DeviceId           : <redacted-guid>
Thumbprint         : <redacted-thumbprint>
Cert Validity      : 2026-01-29 -- 2036-01-29
KeyContainerId     : <redacted-guid>
KeyProvider        : Microsoft Platform Crypto Provider
TpmProtected       : YES
DeviceAuthStatus   : FAILED - Device is either disabled or deleted
```

**Finding:** The device is Azure AD Joined and Domain Joined with a valid TPM-backed certificate, but `DeviceAuthStatus` shows `FAILED` — meaning the device *object* is disabled or deleted in Entra ID, which blocks Intune enrollment despite a healthy local join state.

## Related Incident Handled Same Day
Following a driver-update rollout, several PCs lost printer connectivity due to missing/outdated drivers. Logged in as admin and pushed driver updates across the affected fleet to restore printing.

## Root Cause (Working Theory)
The device object is likely stale or disabled in Entra ID — possibly from a prior re-image or duplicate device record — causing Entra device authentication to fail despite a valid local join state and TPM certificate.

## Remediation Plan
- [ ] Check the Entra ID device object for the affected device — confirm disabled/deleted state
- [ ] Re-enable, or delete and re-register, the device object in Entra ID
- [ ] Re-run `dsregcmd /leave` and rejoin if the object was deleted
- [ ] Confirm Intune enrollment completes after the Entra fix
- [ ] Verify Windows Defender status and BitLocker encryption post-enrollment

## Skills Demonstrated
Entra ID device identity troubleshooting · TPM attestation review · Intune enrollment diagnostics · Cross-platform correlation (Intune / Entra / Azure / Defender) · Incident triage under time pressure (parallel driver-rollout fix)

## Cert & Career Relevance
This is core AZ-104/AZ-500 territory: device object lifecycle in Entra ID, TPM-backed certificate attestation, and conditional access prerequisites. It's a strong interview example of diagnosing a device auth failure with `dsregcmd`, correlating identity state across Intune/Entra/Defender, and managing a parallel mass-incident (broken printer drivers) under time pressure — a natural extension of the "fixing broken desktops" origin story into cloud identity and device security.

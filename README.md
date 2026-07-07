# IT Support & Cloud Identity Runbooks

A collection of real-world desktop support, identity management, and troubleshooting runbooks documented during my work as an IT Technician / Desktop Support engineer, generalized for public sharing.

**Author:** Collins Gora
**Background:** Self-taught IT professional, no formal degree. Certified in CompTIA Security+, Network+, AZ-900, SC-900, MS-900, and Microsoft Applied Skills (AD DS). Currently pursuing AZ-104 → AZ-500 → SC-200 on the path to becoming a Cloud Security Engineer.

## Why this repo exists

Every entry below started as a real support ticket. I document my work daily — partly for accountability, partly because writing down *why* something broke and *how* I fixed it is the same skill used in incident response and security operations, just applied to an end-user's laptop instead of a SOC dashboard.

> **Note on sanitization:** All hostnames, IP addresses, domain names, employee names, and asset identifiers in these runbooks have been replaced with generic placeholders. The technical steps, commands, and troubleshooting logic are unchanged and reflect real work performed. No real company or personal data appears in this repository.

## Runbooks

| # | Runbook | Skills Demonstrated | Cert Alignment |
|---|---|---|---|
| 01 | [BSOD Troubleshooting](./01-bsod-troubleshooting-runbook) | Root cause analysis, Windows internals, structured diagnostics | Security+, Network+ |
| 02 | [Workstation Onboarding](./02-workstation-onboarding-runbook) | Domain join, endpoint security (BitLocker/EDR), imaging, licensing | AZ-104, Security+ |
| 03 | [JML Identity Lifecycle](./03-jml-identity-lifecycle-runbook) | AD provisioning, Entra ID sync, RBAC, M365 licensing | AZ-104, SC-200 |
| 04 | [Entra Device Enrollment Failure](./04-entra-device-enrollment-troubleshooting) | Entra ID device identity, Intune, TPM attestation, conditional access | AZ-104, AZ-500 |

## Origin Story

I got into IT the way most people don't expect — by fixing broken desktops with no formal training and no degree. Working through machines that wouldn't boot taught me the mindset I still use today: read the error, ask the right questions, isolate the variable, verify the fix, document it properly. Every runbook in this repo is that same discipline, just applied to progressively more complex systems — from a single desktop to full identity and device lifecycle management across Active Directory, Entra ID, and Intune.

## Contact

Open to Junior/Associate Cloud Security and SOC Analyst roles. [LinkedIn](#) · [Notion Portfolio](#)

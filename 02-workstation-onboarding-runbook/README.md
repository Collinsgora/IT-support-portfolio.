# End-to-End Workstation Onboarding & Endpoint Security Runbook

## Overview

Full new-device onboarding process for a corporate end user, covering local security baseline, data migration from the old device, domain join, endpoint security stack (BitLocker, EDR), drive mapping, mailbox configuration, software deployment, licensing, and printer setup — plus same-day ad-hoc AD account support handled in parallel.

> All hostnames, IPs, domains, and identifiers below are generic placeholders. The workflow and commands reflect a real onboarding performed on a corporate Windows fleet.

## 1. Local Account & Security Baseline
1. Removed all pre-existing local user accounts from the machine.
2. Reset the local **Administrator** password to the org's default password standard.

## 2. Data Preservation (Before Rebuild/Reimage)
1. Backed up the user's **PST/OST** Outlook data files.
2. Copied business data from the old device to the new one.
3. Copied **Desktop** and **Favorites** folders across.
4. Documented all installed applications on the old machine for replication on the new one.

## 3. Machine Identity
1. Renamed the computer per asset-tag naming convention.
2. Ran the domain-join script and joined the machine to `CorpDomain.local`.

## 4. Endpoint Security Stack
1. Installed updated **endpoint security client** (AV + Drive Encryption).
2. Enabled **BitLocker**, verified with:
   ```powershell
   manage-bde -status
   ```
3. Ran `reagentc.exe /enable` and confirmed **Secure Boot** was active in BIOS before enabling BitLocker.
4. BIOS: enabled Secure Boot, set SATA mode to AHCI.
5. Ran the **EDR onboarding script** and verified status:
   ```powershell
   Get-MpComputerStatus
   ```
6. Applied standard cert-warning policy per the pre-check list.

## 5. Drive Mapping
1. Mapped the **S: drive** (site shared file server).
2. Mapped the **H: drive** (`\\fileserver\users\<username>\Documents` and `...\Desktop`).
3. Added H: drive locations to **My Documents** and **Desktop** libraries.
4. Set both folders to **Available Offline**.

## 6. Outlook / Email Configuration
1. Imported the user's Outlook OST/PST files.
2. Set **Exchange Cached Mode to 6 months**.
3. Updated the user's email signature.
4. Verified backed-up data landed correctly in the new mail profile.

## 7. Core Software Installs (per SOP)
- 7-Zip
- Adobe Acrobat DC
- Java (latest supported update)
- Email security plugin (e.g. Mimecast-equivalent)
- Enterprise browser (Chrome Enterprise)
- Microsoft Office 64-bit (incl. Teams)
- Corporate VPN client (SSO-enabled, custom port profile)
- .NET Framework 3.5 (via Windows Features) + latest Windows Updates
- SCCM/Configuration Manager client + endpoint management agent — verified visible in console

## 8. Licensing & Cloud Identity
1. Activated Office and Windows using the enterprise licence key.
2. Checked Azure AD join status:
   ```powershell
   dsregcmd /status
   ```
3. Where needed: cleared a stale device record via `dsregcmd /leave` then `dsregcmd /join`.

## 9. Application Defaults & Trust Settings
1. Set default programs (Office suite, Outlook, browser, PDF reader).
2. Disabled Protected View in Word/Excel/PowerPoint via the Trust Centre (per internal policy).

## 10. Printing
1. Reinstalled the network printer via the print-queue server.

## 11. Device-Specific Power Settings (laptop)
1. Set Battery Health Manager to "Minimize battery health" via BIOS.
2. Updated BIOS and drivers to latest vendor-supported versions.

## 12. Regional & Local Settings
1. Set region, currency, decimal symbol, and language per company standard.
2. Applied standard local firewall policy per SOP.

## 13. Permissions
1. Added the standard workstation group to local Administrators (per policy).

## 14. Connectivity Extras
1. Migrated SIM card from old device to new, where applicable.

## 15. Documentation
1. Logged in as the end user to confirm a clean first login.
2. Completed asset/ownership documentation for the device.

---

## Additional Ad-Hoc Support (Same Day)
- **AD account unlock** — resolved a locked-out user account in Active Directory.
- **Password reset** — reset the same user's AD password and confirmed login.
- **Label-printing software licensing** — resolved a licensing issue on a business-critical desktop app.

## Skills Demonstrated
Active Directory administration · Endpoint security (BitLocker, EDR) · Azure AD/Entra device state (`dsregcmd`) · SCCM/endpoint management · M365 licensing · Structured SOP-driven documentation

## Cert & Career Relevance
Direct hands-on coverage of AZ-104 domains: identity, device management, and security baseline configuration. This is the kind of end-to-end onboarding checklist that translates directly into device compliance and conditional access work at the SC-200/AZ-500 level.

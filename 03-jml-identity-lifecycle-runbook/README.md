# JML (Joiner-Mover-Leaver) New User Identity Lifecycle Runbook

## Overview

End-to-end onboarding of a new starter: Active Directory account provisioning, Microsoft 365 licensing and mailbox setup, domain-joining the workstation, drive mapping, and configuring specialized label-printing hardware. This is identity lifecycle management from on-prem AD through to cloud license assignment.

> All identifiers below are generic placeholders. The process and commands reflect a real JML workflow performed in a corporate AD/Entra environment.

## 1. New User (Joiner) Account Setup in Active Directory
1. Received the JML ticket confirming the new starter's details: name, job title, department, site, manager, start date.
2. Located the correct **Organisational Unit (OU)** in ADUC matching the site/department (ensures correct GPOs apply).
3. Created the AD user account:
   - Logon name in `firstname.surname` format
   - Temporary password, **"User must change password at next logon"** enabled
4. Populated AD attributes: display name, department, job title, manager, office/site, phone.
5. Added the account to the correct **Security Groups** (site access, department distribution list, app access groups) so downstream permissions inherit correctly rather than being set individually.
6. Verified the account replicated across Domain Controllers before proceeding — avoids a race condition where machine join or mailbox creation fails because the account hasn't propagated yet.

## 2. Microsoft 365 Apps & Mailbox Access
1. Confirmed the account synced to the cloud via **Entra Connect** (on-prem AD → Entra ID identity sync).
2. Assigned the correct **M365 licence SKU** per the user's role.
3. Confirmed **mailbox provisioning** completed in Exchange Online.
4. Set the primary SMTP address per naming convention.
5. Added the user to relevant **Distribution Lists** and **Shared Mailboxes**.
6. Verified MFA/Security Defaults would prompt correctly on first login.
7. Confirmed access to Outlook, Word, Excel, PowerPoint, Teams, OneDrive per assigned licence.

## 3. Machine Setup — Domain Join
1. Confirmed the workstation was on the correct build image.
2. Renamed the machine per asset-tag convention.
3. Joined the machine to `CorpDomain.local`.
4. Restarted and confirmed the machine object appeared in the correct **Computers OU**.
5. Logged in with the new user's credentials to confirm a clean first-time domain profile (GPOs applying, no login errors).
6. Ran:
   ```powershell
   gpupdate /force
   ```
   to confirm drive mappings, security settings, and printer deployment applied immediately.

## 4. Drive Mapping
1. Mapped the **S: drive** — site shared file server.
2. Mapped the **H: drive** — user's redirected personal storage (`Documents`, `Desktop`).
3. Added both to Windows libraries so File Explorer/"Save As" defaults to network storage (protects data if the machine is later wiped/replaced).
4. Set both to **Always Available Offline**.
5. Confirmed mappings persisted after reboot (delivered via GPO/logon script rather than a manual one-off mapping).

## 5. Specialized Label Printer Configuration (Label Software + Thermal Printer)
1. Confirmed label-printing software was installed on the user's machine.
2. Connected the thermal label printer and installed the matching driver.
3. Ran a **test print directly from the driver utility** (bypassing the label software) to isolate hardware/driver issues from software/licensing issues.
4. Configured the printer-driver mapping inside the label software.
5. Verified the software's licence/seat activation was valid for this workstation.
6. Loaded the correct label template (size + DPI matched to the printer's print head resolution) to avoid oversized, cropped, or misaligned labels.
7. Printed a **test label end-to-end** through the software (not just the driver) to confirm the full software → driver → hardware chain.
8. Confirmed print quality, alignment, and barcode scannability with the user before closing the ticket.

## Skills Demonstrated
Active Directory provisioning · Entra ID / Azure AD Connect sync · RBAC via security groups · M365 licensing & Exchange Online · GPO-based drive mapping · Hardware/driver isolation troubleshooting

## Cert & Career Relevance
A JML workflow is identity lifecycle management end-to-end — from on-prem AD provisioning, through Entra ID sync, to licence assignment and group-based access control. This maps directly onto AZ-104 (identity & RBAC) and SC-200 (identity protection, access reviews) exam objectives, and is a strong "I manage the full identity lifecycle" interview story — pairing naturally with a broken-desktop-support origin story as the throughline of a career moving from hardware fixes to full identity and security posture management.

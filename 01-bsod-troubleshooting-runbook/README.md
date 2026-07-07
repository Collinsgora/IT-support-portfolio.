# Windows BSOD (Blue Screen of Death) Troubleshooting Runbook

## Overview

The Blue Screen of Death (BSOD) is Windows' way of saying: *"Something serious went wrong, so I stopped the computer to prevent damage."* Think of it like a car engine automatically shutting off when it detects a major problem — it's a protective mechanism, not a random crash.

This runbook documents the structured desktop-support method I use to diagnose and resolve BSOD incidents, from initial triage through advanced crash-dump analysis.

## Step-by-Step Method

### 1. Read the Stop Code
The BSOD displays a **Stop Code** that narrows down the cause, e.g.:
- `MEMORY_MANAGEMENT`
- `CRITICAL_PROCESS_DIED`
- `SYSTEM_SERVICE_EXCEPTION`
- `IRQL_NOT_LESS_OR_EQUAL`
- `PAGE_FAULT_IN_NONPAGED_AREA`

### 2. Interview the User
- Did anything happen right before the crash?
- New software or hardware installed recently?
- Does it happen every boot, or randomly?
- Was there a recent Windows Update?

### 3. Boot into Safe Mode (if Windows won't start)
1. Power on, then force-shutdown at the Windows logo. Repeat 3x to trigger **Automatic Repair**.
2. Go to **Troubleshoot → Advanced Options → Startup Settings → Restart**.
3. Press **4** for Safe Mode (loads only essential drivers, isolating third-party driver issues).

### 4. Match the Stop Code to a Likely Cause

| Stop Code | Usually Means |
|---|---|
| MEMORY_MANAGEMENT | Faulty RAM |
| CRITICAL_PROCESS_DIED | Corrupted Windows system files |
| PAGE_FAULT_IN_NONPAGED_AREA | Bad RAM or disk errors |
| SYSTEM_SERVICE_EXCEPTION | Driver problem |
| IRQL_NOT_LESS_OR_EQUAL | Driver or hardware conflict |
| KMODE_EXCEPTION_NOT_HANDLED | Faulty drivers |
| WHEA_UNCORRECTABLE_ERROR | Hardware failure (CPU, SSD, motherboard) |

### 5. Repair Windows Files
```powershell
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth
sfc /scannow   # re-run to confirm the fix took
```

### 6. Check the Hard Drive
```powershell
chkdsk C: /f /r
```
Schedule the scan on restart (`Y`), then reboot.

### 7. Test RAM
```
Win + R → mdsched.exe → Restart now and check for problems
```

### 8. Check Device Manager
Look for yellow warning icons or unknown devices; update/reinstall the flagged driver.

### 9. Roll Back Recent Changes
If the BSOD started right after a Windows Update, new software, or a new driver — remove it and retest. Often the fastest fix in a standardized corporate fleet.

### 10. Update Drivers (priority order)
1. Graphics
2. Network
3. Storage
4. Chipset

### 11. Check Event Viewer
`Win + X → Event Viewer → Windows Logs → System`, filter for **Critical / Error / BugCheck** — often names the exact driver responsible.

### 12. Analyze Minidump Files (Advanced)
Crash dumps are stored in `C:\Windows\Minidump`. Tools like **WinDbg** or **BlueScreenView** identify the exact driver/component that triggered the crash — the deepest level of root-cause diagnosis.

## Common Root Causes
Faulty RAM · Corrupted system files · Outdated/bad drivers · Failing SSD/HDD · Overheating CPU/GPU · Bad Windows update · Malware · BIOS/firmware issues · Failing motherboard or PSU

## Ticket Documentation Template
```
Stop Code:
When it occurs:      (startup / random / after specific action)
Recent changes:      (update / new hardware / new software)
Steps taken:
Root cause identified:
Resolution:
Follow-up needed:    (Y/N)
```

## Why This Matters for Security Operations
This troubleshooting method — gather info → isolate the variable → verify the fix → document — is the same structured approach used in SOC and incident response work, just applied to an endpoint instead of a network alert. Root-cause analysis is root-cause analysis, whether the "incident" is a crashed desktop or a security event.

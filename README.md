<div align="center">

# Windows 11 Windows Update Repair

### Controlled reset of Windows Update caches and components — with Insider changes disabled by default.

[![Windows](https://img.shields.io/badge/Windows-11-0078D4?logo=windows&logoColor=white)](#requirements)
[![PowerShell](https://img.shields.io/badge/PowerShell-Administrator-5391FE?logo=powershell&logoColor=white)](#quick-start)
[![Safety](https://img.shields.io/badge/Safety-Insider%20Reset%20Opt-in-2EA44F)](#insider-option)
[![License](https://img.shields.io/badge/License-GPL--3.0-2EA44F)](LICENSE)

<p>
  <a href="WIN11_REPAIR_WU_V1b.ps1">
    <img alt="Open the repair script" src="https://img.shields.io/badge/View-Repair%20Script-5391FE?style=for-the-badge&logo=powershell&logoColor=white">
  </a>
  <a href="#read-before-running">
    <img alt="Read safety notes" src="https://img.shields.io/badge/Read-Safety%20Notes-C62828?style=for-the-badge&logo=github&logoColor=white">
  </a>
</p>

**Reset Windows Update safely, preserve the original service configuration, and keep Personal files untouched.**

</div>

---

## ⚡ What it does

The script stops the Windows Update, BITS, Cryptographic, Delivery Optimization, and MSI services, moves the component caches to a timestamped backup directory, recreates clean cache directories, clears the BITS queue, and restores the services and their original startup modes.

| Step | Result |
|---|---|
| 🧩 Service control | Stops and restarts Update-related services. |
| 📦 Cache reset | Moves `SoftwareDistribution` and `catroot2` under a timestamped backup directory. |
| 🧹 BITS queue | Removes BITS transfer queue database files. |
| 🛡️ Component state | Preserves `WinSxS\pending.xml` instead of deleting it. |
| 🔁 DLL registration | Re-registers discovered Windows Update DLLs with short process timeouts. |
| 🚀 Update detection | Starts an update scan through `UsoClient.exe`. |

It does not run SFC or DISM and does not reinstall Windows or remove personal files.

## ▶️ Quick start

Open PowerShell as Administrator and run:

```powershell
.\WIN11_REPAIR_WU_V1b.ps1
```

The default run **does not change Windows Insider enrollment**.

## 🔐 Insider option

The original script reset registry enrollment to Canary / Mainline / External. This is now opt-in:

```powershell
.\WIN11_REPAIR_WU_V1b.ps1 -ResetInsider
```

To explicitly keep the default safe behavior:

```powershell
.\WIN11_REPAIR_WU_V1b.ps1 -SkipInsiderReset
```

`-ResetInsider` changes `HKLM\SOFTWARE\Microsoft\WindowsSelfHost` after exporting it. Verify the desired Insider channel after reboot. Do not use this option unless you understand the enrollment impact.

## 📋 Read before running

- Run as Administrator.
- Save work and create a restore point if the machine contains an important servicing state.
- Stopping services or moving caches may temporarily interrupt Windows Update, Store updates, or MSI servicing.
- Re-registration can report individual failures; review the console output and backup folder.
- A reboot is recommended before checking Windows Update again.

## 🛡️ Backup and recovery

The script creates:

```text
C:\WU_Repair_Backup_<timestamp>\
```

It exports WindowsSelfHost only when Insider reset is enabled. Cache folders are moved instead of deleted, so an earlier state is recoverable if the move succeeds. If a move fails, the original cache remains in place and the script continues.

## 📦 Repository contents

| File | Description |
|---|---|
| [`WIN11_REPAIR_WU_V1b.ps1`](WIN11_REPAIR_WU_V1b.ps1) | Windows Update and optional Windows Insider repair script. |
| [`LICENSE`](LICENSE) | GNU General Public License v3.0. |

## Requirements

- Windows 11.
- Administrator privileges.
- PowerShell 5.1 or newer.

---

<div align="center">

### 🛡️ Preserve the system state. Reset only what is necessary.

**Windows Update · BITS · Cryptographic Services · Windows Insider**

</div>

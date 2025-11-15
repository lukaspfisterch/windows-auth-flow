# Windows Authentication Flow Framework

## Overview
A compact, engineering-grade framework to understand and troubleshoot the Windows authentication chain — from PRT to Conditional Access — across client and cloud boundaries.

This repository distills lessons from years of real-world support work (Outlook / Teams / Edge sign-in issues) into a clear, layered model (A–D).  
It provides structure, vocabulary, and safe intervention levels for 1st–3rd line support.

---

## Origin
The project began as a series of PowerShell repair tools (`OutlookReset.ps1`, `TeamsReset.ps1`, `WAMCleanup.ps1`).  
All targeted different symptoms — yet every root cause pointed to the same source:  
**authentication state corruption**.

From that observation emerged this framework:  
**Tools → Diagnostics → Architecture.**

---

## Layer Model (A–D)

| Layer | Scope | Description | Typical Reset / Check |
|-------|--------|--------------|------------------------|
| **A** | Identity Token | Device join integrity, PRT validity, AAD trust | `dsregcmd /status`, rejoin device |
| **B** | WAM Store | Web Account Manager state (user + device contexts) | WAM reset or `runDll32.exe keymgr.dll,KRShowKeyMgr` |
| **C** | App Tokens | MSAL / OneAuth caches, per-app token persistence | Targeted cache cleanup |
| **D** | Session Layer | Edge WebView2 and cookie state | Browser or WebView2 reset |

Each layer can fail independently — the framework maps **symptoms → layer → safe reset path**.

---

## Support Competence Zones

| Level | Responsibility | Access Scope |
|--------|----------------|--------------|
| 1️⃣ 1st Line | Guided reset via PowerShell menu (A–D) | User context only |
| 2️⃣ 2nd Line | Layer-specific resets & validation | Local admin, logs |
| 3️⃣ 3rd Line | Deep analysis, CA & policy flow correlation | Directory / AAD access |

This mapping clarifies *who touches what* — reducing risk and ensuring repeatability.

---

## Visual Overview

```text
+-----------------------------------------------------------+
|                 WINDOWS AUTHENTICATION FLOW               |
+-----------------------------------------------------------+
|  A  |  B  |  C  |  D  |                                   |
| PRT | WAM | MSAL/OneAuth | WebView2/Cookies | -> AAD/CA  |
+-----------------------------------------------------------+
      ↓       ↓          ↓                ↓
  Device  Account   App Token        Session State
  Trust   Context   Integrity        Persistence
```

![Authentication Flow Overview](diagrams/auth-flow-overview.svg)

![Common Failure Map](diagrams/failure-map.svg)

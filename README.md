<img src="logo.png" width="96" alt="GitWorkflow">

# GitWorkflow - releases

Download and update channel for **GitWorkflow**, an internal desktop Git GUI (Tauri + Rust)
with first-class worktrees and built-in terminals running Claude Code.

This repository holds nothing but release artifacts. It is public because the app's built-in
updater needs somewhere it can reach without a token - not because the app is offered to the
public.

## Terms

GitWorkflow is an internal tool, not open-source software and not a product. It is intended for
the copyright holder and the people working with him, who may install and use it for their own
work. Making the installers reachable here does not license anyone else to use, redistribute,
mirror or repackage them.

The app is provided **as is**, with no warranty and no support commitment. It drives git, and
git commands can destroy work; keep backups.

Copyright (c) 2026 F. de Groot. All rights reserved. The libraries the app bundles carry their
own licences, listed in the application's `NOTICE.md`.

## Manual

New here? **[Read the manual](MANUAL.md)** - installing, the views, the daily git actions,
worktrees and the terminals, on one page. It is republished with every release, so it describes
the version on the releases page.

## Install

Take the installer from the **[latest release](../../releases/latest)** - under **Assets**,
`GitWorkflow_x.y.z_x64-setup.exe`.

| Asset | Purpose |
|---|---|
| `GitWorkflow_x.y.z_x64-setup.exe` | **The installer - this is the one you want** |
| `GitWorkflow_x.y.z_x64_en-US.msi` | Alternative installer for managed deployment (Intune/GPO) |
| `*.sig` | Signatures, verified by the built-in updater |
| `latest.json` | Update manifest, polled by the built-in updater |

Run the setup. Windows SmartScreen may warn on first install because the installer is not
Authenticode-signed - choose *More info -> Run anyway*. The app installs per-user; no
administrator rights required.

> On a Windows 11 machine with **Smart App Control** enabled, unsigned binaries are blocked
> rather than warned about, with no "Run anyway". Until signing is in place, GitWorkflow cannot
> be installed or updated on such a machine.

## Updates

GitWorkflow checks this repository for updates on startup (and via **Settings -> About -> Check
for updates**) and installs them itself. Every update is cryptographically signed and verified
before installation, so there is no need to come back here.

## Source

The source lives in a separate private repository. This one only hosts the artifacts.

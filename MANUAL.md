<img src="https://raw.githubusercontent.com/f-de-groot/git-workflow-releases/main/logo.png" width="72" alt="GitWorkflow">

# GitWorkflow - user manual

A desktop Git client with built-in terminals: the commit graph, everything git does every day,
a working directory per branch, and terminals that can run Claude Code next to the code they
are about to change.

This page describes the version currently on the
[releases page](https://github.com/f-de-groot/git-workflow-releases/releases/latest). It is
published again with every release, so what you read here is what you can download.

GitWorkflow is an internal tool, provided as is. It drives git, and git commands can destroy
work, so keep backups. Found a bug, or missing something? Tell Franklin - the source repository
is private, so there is no issue tracker you can reach.

---

## Install

1. Open the [latest release](https://github.com/f-de-groot/git-workflow-releases/releases/latest)
   and take `GitWorkflow_x.y.z_x64-setup.exe` from **Assets**.
2. Run it. Windows SmartScreen warns because the installer is not signed yet: choose
   **More info -> Run anyway**.
3. It installs per user, so no administrator rights are needed.

The `.msi` next to it is the same app for managed deployment (Intune/GPO); the `.sig` files and
`latest.json` are for the updater and not something you download by hand.

> On a machine with **Smart App Control** switched on, Windows blocks unsigned installers
> outright instead of warning. Until signing is in place, GitWorkflow cannot be installed there.

## Updates

GitWorkflow checks for a new version on startup and installs it itself; every update is signed
and verified before it is applied. You can also check from **Settings -> About -> Check for
updates**, and switch the startup check off in the same place. There is no need to come back to
the releases page after the first install.

## What you need on your machine

| | |
|---|---|
| **git** | **Required.** GitWorkflow ships no git of its own: every action runs the `git` on your `PATH`. Install with `winget install Git.Git`, then check with `git --version` in a fresh terminal. Without it the app starts but every action fails with *"git was not found on PATH"*. |
| **Claude Code** | Optional, for the ✨ buttons and the Claude sessions. It is the `claude` CLI, not the desktop app, installed globally and signed in once on this machine, with a subscription or API credit. **Settings -> Claude** has an **Install Claude Code** button and an **Is Claude installed?** check that tells you which of those four is missing. No Claude? Switch **Use Claude** off and every Claude entry point disappears; the rest of the app is untouched. |
| **GitHub CLI (`gh`)** | Optional. Only used as a fallback for listing pull requests when you have not signed in to a hosting account. |

> A desktop app inherits the `PATH` from the moment it started. Install something while
> GitWorkflow is open and you may have to restart it before the app sees it.

## Sign in

**Settings -> Workspace** holds your identities. A **workspace** is a named identity with its own
hosting accounts, so a work account and a private account can live side by side; switch
workspaces and the next clone, fetch, pull or push authenticates as that account.

Per workspace you can connect **GitHub** and **Bitbucket**, both through **Sign in with …**,
which opens your browser. You sign in at the provider itself, including your organisation's
SSO/2FA; GitWorkflow never sees your password, and tokens are stored in Windows Credential
Manager. Bitbucket tokens expire after two hours and are refreshed automatically.

Signing in gets you:

- **push and pull over HTTPS without an SSH key** - no more credential popups for these hosts;
- **Browse GitHub… / Browse Bitbucket…** in the clone dialog, a searchable list of your repositories;
- pull requests **inside the app**: the list, the diff, creating one, and merging it;
- creating a new repository at the host from **Repos -> Create repo…**.

Bitbucket team workspaces cannot be listed by an app any more, so fill in their slugs
(comma separated, the part after `bitbucket.org/`) in the same panel.

SSH remotes keep working through your own `~/.ssh/config` and agent. You can pick a specific key
per workspace and provider, and it is used only where HTTPS is not an option. A remote can be
switched over with **right-click the remote -> Switch to HTTPS**.

## The window

- **Topbar:** the repo and current branch on the left; on the right the commit search, **fetch /
  pull ▾ / push**, **↺ undo ▾**, **📚 Repos**, **⟳ Refresh** and **⚙ Settings**.
- **Repository tabs** under it: every open repo is a tab, and they are remembered between sessions.
- **Sidebar:** LOCAL and REMOTE branches, STASHES, PULL REQUESTS, TAGS, and PROJECT (the file tree).
- **Views**, switched from the bar or with a shortcut: **Git** (Ctrl+1), **Claude agent**
  (Ctrl+2), **Explorer** (Ctrl+3), **Terminal** (Ctrl+4), **Issues** (Ctrl+5) and **Worktrees**
  (Ctrl+6). Views you never use can be switched off in **Settings -> Various**.
- **Terminal dock** at the bottom, on top of everything else. `` Ctrl+` `` opens and closes it;
  closing it ends nothing.

Diffs, settings and opened files appear as a page over the graph with a **✕** at the top right.

## Open, clone or create a repository

**📚 Repos** has four entries: **Open repo…** (a folder that is already a repository),
**Clone repo…**, **Create repo…** (a new repository at GitHub or Bitbucket, with a **Clone now…**
button afterwards) and **Initialise folder…** (`git init` on a folder you already have).

Cloning shows git's own progress and opens the clone when it is done. Prefer the HTTPS URL: then
the account you signed in with does the authenticating.

## The commit graph

Five columns: **branch/tag labels** (tinted in the lane colour of their branch, local and remote
merged into one label with a monitor and a cloud icon), the **graph** itself, the **message**,
the **short hash** and the **date**. Drag the separators to resize; the widths are remembered.

- **Click a commit** to open its files and diff over the graph. **All files** shows the whole
  commit; click one file for just that file. **Side by side / Unified** is a toggle at the top
  right, and side-by-side highlights the changed words within a line.
- **Ctrl-click / Shift-click** selects several commits: you get one list of every file they
  touch, with **×2**, **×3** behind a file more than one of them changed.
- **Click a branch label** for a detail panel with that commit and its files; **double-click** it
  to check the branch out (uncommitted work is stashed first, and a yellow **⇣ pop stash** button
  appears in the topbar to put it back).
- **🔍 Search** filters the graph live on message, author, email, hash or stash, with
  **Enter / Shift+Enter** to jump between matches.
- The repo is watched for changes, so commits, checkouts and edits from a terminal show up by
  themselves within about half a second. **⟳ Refresh** is still there.

## Committing

Uncommitted work appears as a row **Uncommitted changes (N)** above the graph. Click it:

- Left: **STAGED** and **CHANGES**. Hover a file for **+ stage** / **− unstage**, or use stage
  all / unstage all. Right-click a file for **Stash this file** or **Discard changes**.
- **Per hunk:** click a file and use **+ Stage hunk** on a hunk header in the diff. That is how
  you split one file over two commits.
- **⇡ Stash all** at the top puts everything aside in one stash, untracked files included.
- Type a **Commit summary** and press **Commit**. The counter next to it (`12/72`) turns yellow
  past 72 characters, because the first line is what every log and graph shows. Tick
  **Description** for a body under it.
- **✨ AI commit message** writes the message from the staged diff, in English, in the style of
  your recent commits.

## Branches, merging and rebasing

Right-click a branch - in the sidebar or on its label in the graph, it is the same menu - for
pull, push, **Start pull request**, checkout, **Create branch here**, rename, pin, hide or solo
in the graph, delete (also several at once after Ctrl-clicking them), **Edit commit message**,
**Revert commit**, **Reset to this commit** and **Create worktree from this commit**.

**Merging and rebasing is drag and drop:** drag a branch onto another branch and pick **Merge**,
**Rebase**, **Interactive rebase** or **Reset** from the menu that always appears. Dragging a
commit onto a branch offers **Cherry-pick** or **Reset**. Destructive actions ask first, and the
confirmation warns you when a merge is going to conflict.

**Interactive rebase** opens a window with the newest commit on top: choose **Pick**, **Reword**,
**Squash** or **Drop** per commit (or press **P / R / S / D**), reorder with ↑/↓, then
**Start rebase**.

## Conflicts

A merge, rebase, pull or cherry-pick that conflicts puts a **yellow bar** above the views with
the conflicting files. Click one and the **conflict editor** opens: tick **per line** which side
to keep, use *All current / All incoming / All both*, edit the output at the bottom freely, and
**Save & stage** writes it.

With Claude available there is also **✨ Resolve with Claude** for one file, **✨ Resolve all with
AI** for every conflicting file at once, and **✨ Finish rebase with AI**, which keeps resolving
and continuing until the whole rebase is done. Those runs happen in a panel at the bottom right,
so you can carry on working; if Claude cannot resolve a file the whole rebase is aborted and your
branch is exactly where it was.

Or resolve them in the terminal, or press **Abort merge / Abort rebase** in the bar.

## Undo

**↺ undo ▾** in the topbar reverses the last thing the app did in this repository: a commit
(`reset --soft`, so the files come back staged), a merge or rebase, creating or deleting a
branch, or dropping a stash. The ▾ shows the last ten; you undo one step at a time. Has the
repository moved on since, the undo refuses and changes nothing. The list is not persisted: it
starts empty when the app starts.

## Worktrees

A **worktree** is a second working directory of the same repository with its own checked-out
branch - its own files, its own terminals and its own Claude session. Working on three branches
at once without stashing or switching is the point of the whole app.

- **Worktrees** (Ctrl+6) lists them. **+ New worktree** takes an existing branch name or a new
  one; with a default folder set in **Settings -> Repository** it is created without further
  questions. Click a row to switch to it.
- New worktrees can copy the things git does not track but your app needs to run -
  `vendor`, `public/build`, `.env` - from the worktree you came from; the list is in
  **Settings -> Repository -> Copy into a new worktree**.
- **🌐 Preview** opens the local site of the worktree you are in, at the address your local site
  server actually serves it on. Optionally the app can create and remove that site per worktree
  (**Settings -> Repository -> Local site server**).
- **⎋ Close worktree (PR/Merge)** is how work leaves a worktree: commit, push, then either open a
  **pull request** or **merge locally**, then clean up the worktree and (optionally) the branch.
  It stops at the first thing that fails, with nothing removed yet.
- **✕ Remove worktree** only removes the working directory and keeps the branch and its commits.

New worktrees are created from the repository itself, never from inside another worktree, and a
branch can only be checked out in one worktree at a time.

## The Explorer

**Explorer** (Ctrl+3) is the file tree on the left and the files you opened on the right, as tabs.

- **Click a file** to open it; several files stay open at once, each with its own scroll position
  and undo history. **Ctrl+W** closes the one on screen, **Ctrl+Tab** walks to the next.
- **Files are editable right away** and **save themselves**: 5 seconds after you stop typing, and
  immediately when you leave the tab or the view, when the window loses focus, when the tab closes
  and before any git command that reads your files. **Ctrl+S** writes now. The badge next to the
  file name says where it stands - **Unsaved changes**, **Saving…**, **Saved**.
- A file that changed on disk while you were editing it is never overwritten: a bar appears with
  **Reload from disk** and **Overwrite**.
- **Ctrl+click a name or an import** jumps to where it is defined. The column beside the code
  lists the symbols in the file and filters as you type.
- **Right-click a file or folder** for new file, new folder, rename, delete (to the recycle bin),
  copy path, show in Explorer, and file history or blame. **Drag a row onto a folder** to move it.
- **Paste a file from the clipboard:** copy a file anywhere (Windows Explorer, a mail, a
  screenshot), click the folder in the tree you want it in, and press **Ctrl+V**. It is copied
  into that folder - a file you click hands it to the folder it sits in, the empty space below the
  rows is the repository root, and an existing file is never overwritten. Folders cannot be
  pasted: the clipboard hands over file contents only. A path on the clipboard works just as
  well (**Copy as path** in Windows Explorer, or this tree's own **Copy absolute path**), and
  that one has no size limit.
- **Ctrl+Shift+F** searches the contents of every file in the repository.

## Issues and pull requests

**Issues** (Ctrl+5) shows the issues of the repository as a **List** with a detail panel, or as a
**Board** of the GitHub project whose cards you drag between statuses. **+ New issue** (or the
**+** on a board column, which sets that status) opens a dialog with title, description,
assignee, labels, type and status; nothing is written until you press **Create issue**.

Two buttons hand an issue to Claude: **Pick up in Claude** types it into a session, and **Create
worktree and pick up in Claude** makes the worktree for it first.

**PULL REQUESTS** in the sidebar lists the open ones. Click a PR to see the full diff in the app,
with **Open in browser** and **Merge ▾** (merge, squash or rebase, with a confirmation).

**Right-click a branch -> Start pull request** opens the create dialog: source and target repo
(so a fork works too), the branches, a title and a description, or **✨ Generate title and
description** to have Claude write them from the commits and the diff. Afterwards you get the
URL of the new pull request.

To have a PR reviewed: **drag it from the list onto a Claude tab**. The tab lights up, takes
focus, and the review instruction is typed and sent. Make sure that session is sitting at its
prompt.

## Terminals and Claude

- **+ Terminal** opens a shell (PowerShell on Windows), **+ Claude** opens one that starts
  `claude` straight away. Both open in the repository you have open. Claude tabs come first in
  the tab strip, then the plain terminals; **✕** ends that session.
- Terminals keep running in the background - on another repo tab, in another view, with the dock
  closed. Only **✕** on the tab itself ends a session.
- **Selected text is copied to the clipboard immediately**, so selecting is all it takes.
  Because of that **Ctrl+C is the interrupt again**, even with text selected. **Ctrl+Shift+C**
  copies explicitly, **Ctrl+V** and **Ctrl+Shift+V** paste, and right-click pastes when nothing
  is selected.
- **Quick prompts:** above a Claude session sits a row of buttons with your saved prompts (edit
  them in **Settings -> Claude Prompts**); one click types the prompt and runs it.
- **Prompts panel:** the **Prompts** button in the Claude tab bar slides open a column with
  everything you asked that session, numbered and timed, with **Reuse** and **Copy** per line.
  Claude's answers bury your questions otherwise. After a `/resume` the prompts of the session you
  picked up appear above the ones you gave here, under a **resumed here** line.
- **Drag a file onto a terminal tab** (a screenshot, say) and its path lands on the prompt, ready
  for you to type a question after it.
- Drag the bar between the graph and the dock to resize it. One button always sits in the middle
  of that edge: **Open terminal** when it is closed, **Open graph** when it is open.

## Keyboard shortcuts

| | |
|---|---|
| **Ctrl+1 … Ctrl+6** | Git, Claude agent, Explorer, Terminal, Issues, Worktrees |
| **Ctrl+`** | Open or close the terminal dock |
| **Ctrl+P** | Command palette: fetch, pull, push, stash, check out any branch, switch repo tab, open a worktree, settings, a new terminal |
| **Ctrl+C** | Interrupt what the terminal is running |
| **Ctrl+Shift+C** | Copy the terminal selection (selecting already copies it) |
| **Ctrl+V** | Paste into the terminal, or a copied file into the Explorer tree |
| **Ctrl+S** | Save the file you are editing in the Explorer now |
| **Ctrl+Shift+F** | Search the contents of every file (Explorer) |
| **Esc** | Close a dialog or clear the commit search |

The shortcuts work while you are typing in a terminal; the shell does not see them.

## Settings

- **Workspace** - identities and their GitHub/Bitbucket accounts, and the SSH key per provider.
- **Claude** - **Use Claude** on or off, the command the **+ Claude** tab runs, and the check
  that tells you whether Claude is usable.
- **Claude Prompts** and **Terminal Prompts** - the quick prompt buttons above each kind of session.
- **Repository** - commit signing (GPG or SSH, with **Test signing** that signs a throwaway
  object), the external editor, the default worktree folder, what gets copied into a new
  worktree, the browser preview and the local site server.
- **Various** - theme (System, Dark or Light, applied instantly, terminals included), which views
  and columns are shown, the start view, font sizes per part of the app, and **Export**/**Import**
  of all your settings as one JSON file.
- **About** - version, update check, and a link back to this manual.

## When something goes wrong

**"git was not found on PATH"** - install git, then restart GitWorkflow so it picks up the new
`PATH`.

**A ✨ button does nothing, or Claude is greyed out** - it is almost always one of four: `claude`
is not installed, only the desktop app is installed, you are not signed in on this machine, or
there is no credit. **Settings -> Claude -> Is Claude installed?** says which one.

**A commit fails with `error: cannot spawn : No such file or directory`** - commit signing is on
while the signing program is set to an empty value, usually in your global `.gitconfig`.
**Settings -> Repository -> Commit signing** shows a warning with a button that removes that
empty value. By hand: `git config --global --unset gpg.ssh.program`.

**Windows blocks the installer** - SmartScreen: *More info -> Run anyway*. Smart App Control
blocks it for real; that machine cannot run GitWorkflow until the installers are signed.

**An update will not install** - check **Settings -> About -> Check for updates** for the
message, and otherwise install the latest setup from the releases page over your current
version; your settings are kept.

**The preview button is missing on a worktree** - only a repository that is a website gets one.
For anything else there is nothing to serve.

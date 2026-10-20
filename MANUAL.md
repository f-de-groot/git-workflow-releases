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
| **Claude Code** | Optional, for the Claude buttons and the Claude sessions. It is the `claude` CLI, not the desktop app, installed globally and signed in once on this machine, with a subscription or API credit. **Settings -> Claude** has an **Install Claude Code** button and an **Is Claude installed?** check that tells you which of those four is missing. No Claude? Switch **Use Claude** off and every Claude entry point disappears; the rest of the app is untouched. |
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
- creating a new repository at the host from **Repos -> Create repository…**.

Bitbucket team workspaces cannot be listed by an app any more, so fill in their slugs
(comma separated, the part after `bitbucket.org/`) in the same panel.

SSH remotes keep working through your own `~/.ssh/config` and agent. You can pick a specific key
per workspace and provider, and it is used only where HTTPS is not an option. A remote can be
switched over with **right-click the remote -> Switch to HTTPS**.

## The window

Every button has one place, decided by what it works on:

- **Top row - the repository and the app:** the **repository tabs** on the left - every open repo
  is a tab with the project's favicon (or the app icon when it has none) and its checked-out branch
  next to the name, and they are remembered between sessions. On the right **Repos**, then what
  belongs to the repository on screen - **Preview** for a website, **Worktree** inside a
  worktree - and then **PR review** and **Settings** (click it again, or the close button in
  Settings, to close it). With repo tabs switched off, the repo and current branch are shown
  instead.
- **Second row - the views:** **Git** (Ctrl+1), **Claude** (Ctrl+2), **Explorer** (Ctrl+3),
  **Terminal** (Ctrl+4), **Issues** (Ctrl+5) and **Worktrees** (Ctrl+6) on the left, and on the
  right **the buttons of the view you are in** - always in the same spot, with the view's **+**
  button (**Claude**, **Terminal**, **New issue**, **New worktree**) last. In a narrow window the
  other views show only their icon. Views you never use can be switched off in
  **Settings -> General**.
- **Git view buttons:** search (the magnifier, or **Ctrl+F**), **Refresh**, **Fetch**, **Pull**,
  **Push**, **Undo**, and **Pop stash** when there is a stash. The arrow next to Pull and Push picks
  what a click on the button does (for Push: push, push and set upstream, or force push with
  lease), and the choice is remembered. With Pull set to fetch, the separate Fetch button goes
  away. A force push always asks first, and the button then reads **Force push** in red. While
  the graph is filtered, a yellow **Only 'X'** or **N branches hidden** button in front of them
  shows everything again.
- **Sidebar** (Git view): local and remote branches, pull requests, tags and stashes.
- **Progress** shows on the button you pressed: Fetch, Pull, Push, Undo, Pop stash, Commit
  and Commit and push turn their icon into a spinner while they run.
- **Status bar** at the bottom: what the app is doing right now (fetching, pulling, refreshing,
  with a green check when it is done), and on the right the Claude model and your plan usage
  (see *Terminals and Claude*).

Diffs and opened files appear as a page over the graph with a close button at the top right. A
menu closes when you click anywhere outside it or press **Esc**, and the arrow keys move through
it. Right-clicking only ever opens the app's own menus - never the browser menu with *Reload*,
which would end every running session.

**Dialogs** all work the same way: **Esc** or a click beside the dialog cancels it, the main
action is the blue button on the right, and a dialog in which you typed something asks before it
throws that away. While a dialog is busy (cloning, saving) it stays open until it is done.
Messages appear at the top right, below any open dialog; after deleting a branch or a stash the
message has an **Undo** button.

## Open, clone or create a repository

**Repos** has four entries: **Open repository…** (a folder that is already a repository),
**Clone repository…**, **Create repository…** (a new repository at GitHub or Bitbucket, with a
**Clone now** button afterwards) and **Initialise a folder…** (`git init` on a folder you already
have). With no repository open, the Git view offers **Open repository…** and **Clone
repository…** as well.

Cloning shows git's own progress and opens the clone when it is done. Prefer the HTTPS URL: then
the account you signed in with does the authenticating.

## The commit graph

Five columns: **branch and tag labels** (tinted in the lane colour of their branch, local and
remote merged into one label with a monitor and a cloud icon), the **graph** itself, the
**message**, the **commit** hash and the **date**. Drag the separators to resize; the widths are
remembered.

- **Click a commit** to open its files and diff over the graph. **All files** shows the whole
  commit; click one file for just that file. **Side by side / Unified** is a switch at the top
  right, and side by side highlights the changed words within a line. A very large diff (over
  5,000 lines or 1 MB) waits for **Show anyway** before it is drawn.
- **Ctrl-click / Shift-click** selects several commits: you get one list of every file they
  touch, with **×2**, **×3** behind a file more than one of them changed.
- **Click a branch label** for a detail panel with that commit and its files; **double-click** it
  to check the branch out (uncommitted work is stashed first, and a **Pop stash** button appears
  among the Git buttons to put it back).
- **Search** (or **Ctrl+F**) filters the graph live on message, author, email, hash or stash,
  with **Enter / Shift+Enter** to jump between matches and **Esc** to clear it.
- The repo is watched for changes, so commits, checkouts and edits from a terminal show up by
  themselves within about half a second. **Refresh** is still there.

## Committing

Uncommitted work appears as a row **Uncommitted changes (N)** above the graph, and its panel opens
on the right by itself whenever nothing else is shown there - so the commit box is simply there
while you work. Clicking a commit shows that commit instead; closing it brings the changes back.
Close the changes panel and it stays closed until the working tree is clean again or you click
the row. In the panel:

- Left: **Staged** and **Changes**, with each file's state as a letter: **M** modified, **A**
  added, **D** deleted, **R** renamed, **?** untracked and a red **U** for a conflict. Hover a
  file for its stage or unstage button, or use **Stage all** / **Unstage all**. Right-click a file
  for **Stash this file**, **Show in Explorer view** (which opens it in the Explorer view at its
  first change) or **Discard changes**.
- **Per hunk:** click a file and use **Stage hunk** on a hunk header in the diff. That is how you
  split one file over two commits.
- **Stash all** at the top puts everything aside in one stash, untracked files included.
- Type a **Summary** and press **Commit** (or **Commit and push**). The counter next to it
  (`12/72`) turns yellow past 72 characters, because the first line is what every log and graph
  shows. Tick **Description** for a body under it.
- **Write message with Claude** writes the message from the staged diff, in English, in the style
  of your recent commits. Which model writes it is set under **Settings -> Claude -> Commit message
  model**.

## Branches, merging and rebasing

Right-click a branch - in the sidebar or on its label in the graph, it is the same menu - for
pull, push, **Create pull request…**, check out, **Create branch here…**, **Copy branch name**,
**Rename branch…**, pin, hide or solo in the graph, **Edit commit message…**, **Revert commit**,
**Create worktree from this commit** and, always in the last group, **Reset to this commit** and
the deletes (also several branches at once after Ctrl-clicking them).

**Merging and rebasing is drag and drop:** drag a branch onto another branch and pick **Merge**,
**Rebase**, **Interactive rebase** or **Reset** from the menu that always appears. Dragging a
commit onto a branch offers **Cherry-pick** or **Reset**. Every confirmation names what it acts on
and says whether **Undo** can take it back; a merge warns you when it is going to conflict.

**Interactive rebase** opens a window with the newest commit on top: choose **Pick**, **Reword**,
**Squash** or **Drop** per commit (or press **P / R / S / D**), reorder with the up and down
arrows, then **Start rebase**.

## Conflicts

A merge, rebase, pull or cherry-pick that conflicts puts a **yellow bar** above the views with
the conflicting files. Click one and the **conflict editor** opens: tick **per line** which side
to keep, use **Use current / Use incoming / Use both**, edit the output at the bottom freely, and
**Save and stage** writes it.

With Claude available there is also **Resolve with Claude** for one file, **Resolve all with
Claude** for every conflicting file at once, and **Finish rebase with Claude**, which keeps
resolving and continuing until the whole rebase is done. Those runs happen in a panel at the
bottom right, so you can carry on working; **Stop** there ends a run at once and leaves the file
it was on untouched. If Claude cannot resolve a file the whole rebase is aborted and your branch
is exactly where it was. Claude reads the whole file but only writes the conflicting parts, so
the rest of the file stays exactly as it was. Which model and effort Claude uses is set under
**Settings -> Claude -> Conflict resolution**; by default it follows your own Claude Code
settings.

Or resolve them in the terminal, or press **Abort merge / Abort rebase** in the bar. Abort and
**Skip commit** ask first: both throw work away.

## Undo

**Undo** among the Git buttons reverses the last thing the app did in this repository: a commit
(`reset --soft`, so the files come back staged), a merge or rebase, creating or deleting a
branch, or deleting a stash. The arrow next to it shows the last ten; you undo one step at a time.
Has the repository moved on since, the undo refuses and changes nothing. The list is not
persisted: it starts empty when the app starts.

## Worktrees

A **worktree** is a second working directory of the same repository with its own checked-out
branch - its own files, its own terminals and its own Claude session. Working on three branches
at once without stashing or switching is the point of the whole app.

- **Worktrees** (Ctrl+6) lists them. **New worktree** takes an existing branch name or a new
  one; with a default folder set in **Settings -> Repository** it is created without further
  questions. Click a row to switch to it.
- New worktrees can copy the things git does not track but your app needs to run -
  `vendor`, `public/build`, `.env` - from the worktree you came from; the list is in
  **Settings -> Repository -> Copy into a new worktree**.
- **Preview** in the top row opens the local site of the worktree you are in, at the address
  your local site server actually serves it on. Optionally the app can create and remove that site
  per worktree (**Settings -> Repository -> Local site server**).
- **Worktree** in the top row, while you are in a worktree, holds the two ways to end it:
  - **Finish worktree…** is how work leaves a worktree: commit, push, then either open a
    **pull request** or **merge locally**, then clean up the worktree and (optionally) the branch.
    It stops at the first thing that fails, with nothing removed yet.
  - **Remove worktree…** only removes the working directory and keeps the branch and its commits.

New worktrees are created from the repository itself, never from inside another worktree, and a
branch can only be checked out in one worktree at a time.

## The Explorer

**Explorer** (Ctrl+3) is the file tree on the left and the files you opened on the right, as tabs.

- **Click a file** to open it; several files stay open at once, each with its own scroll position
  and undo history. **Ctrl+W** (or **Ctrl+F4**) closes the one on screen, **Ctrl+Tab** or
  **Alt+Right** walks to the next and **Ctrl+Shift+Tab** or **Alt+Left** to the previous one.
  **Ctrl+E** lists the open files, the one you looked at last first: **Enter** takes you back to
  the file you were just in, typing filters the list.
- **Files are editable right away** and **save themselves**: 5 seconds after you stop typing, and
  immediately when you leave the tab or the view, when the window loses focus, when the tab closes
  and before any git command that reads your files. **Ctrl+S** writes now. The badge next to the
  file name says where it stands - **Unsaved changes**, **Saving…**, **Saved**.
- A file that changed on disk while you were editing it is never overwritten: a bar appears with
  **Reload from disk** and **Overwrite**, which asks first.
- **Markdown files** (`.md`) open **rendered**: headings, lists, tables, task lists, links and
  coloured code blocks, in the app's theme. **Preview** / **Code** next to the file name (or
  **Ctrl+Shift+V**) switches to the text to edit it; the preview shows what you typed, saved or
  not. A web link opens in the browser, a link to another file in the repository opens it as a
  tab, and an image that cannot be shown here (a path inside the repository, for one) shows its
  alt text instead.
- **Ctrl+click a name or an import** (or put the caret on it and press **Ctrl+B**) jumps to where
  it is defined. The column beside the code lists the symbols in the file and filters as you type.
- **Ctrl+G** goes to a line: type `42`, or `42:7` for a column as well, and press **Enter**.
- **Ctrl+F** finds text in the open file. Every match is marked, the current one stronger;
  **Enter** or **F3** goes to the next one, **Shift+Enter** or **Shift+F3** to the previous one,
  **Aa** matches case and **Esc** closes the bar with the match still selected. Text you selected
  on one line becomes the search. **Ctrl+R** opens the same bar with a replace row: **Replace**
  (or **Enter** in that box) replaces the current match and moves on, **Replace all** does every
  match at once. Both are one **Ctrl+Z** away.
- **Line editing:** **Ctrl+/** comments the selected lines out, or back in when they all are
  comments already (`//`, `#`, `--` or `<!-- -->`, depending on the file type). **Ctrl+D**
  duplicates the line, or the selection when there is one. **Alt+Shift+Up/Down** moves the
  selected lines up or down.
- **Right-click a file or folder** for new file, new folder, rename, copy path, **Show in File
  Explorer**, file history or blame, and last **Delete** (to the Recycle Bin, so it can be restored).
  **Drag a row onto a folder** to move it. The buttons above the tree make a new file or folder and
  show or hide the folders git ignores.
- **Paste a file from the clipboard:** copy a file anywhere (Windows Explorer, a mail, a
  screenshot), click the folder in the tree you want it in, and press **Ctrl+V**. It is copied
  into that folder - a file you click hands it to the folder it sits in, the empty space below the
  rows is the repository root, and an existing file is never overwritten. Folders cannot be
  pasted: the clipboard hands over file contents only. A path on the clipboard works just as
  well (**Copy as path** in Windows Explorer, or this tree's own **Copy absolute path**), and
  that one has no size limit.
- **Press the left Shift twice** (or **Ctrl+Shift+N**) for **Go to file**: type part of a file
  name and pick it from the files of the project. A slash in what you type matches the folder as
  well (`models/user`), and `UserController:42` opens the file at line 42. Tick **Include ignored
  files** (or press the left Shift twice again) to search what git ignores and the dependency
  folders such as `vendor/` and `node_modules/` as well; they are listed after the project's own
  files. The box starts unticked every time, and while it is, the popup says how many matches it
  left out.
- **Press the right Shift twice** (or **Ctrl+Shift+F**) to search the repository in the sidebar:
  file names and the contents of every file, in one list. Ignored files and vendor folders are
  left out here.

## Issues and pull requests

**Issues** (Ctrl+5) shows the issues of the repository as a **List** with a detail panel, or as a
**Board** of the GitHub project whose cards you drag between statuses. **New issue** (or the
**+** on a board column, which sets that status) opens a dialog with title, description,
assignee, labels, type and status; nothing is written until you press **Create issue**. A new
issue starts assigned to you (when you can be assigned in that repository) and without labels.
The detail panel shows the description and the comments formatted the way GitHub does, including
screenshots and other images uploaded to the issue.

Two buttons hand an issue to Claude: **Pick up with Claude** types it into a session, and
**Create worktree and pick up with Claude** makes the worktree for it first. **Close issue** in
the detail panel closes it on GitHub as completed, not planned or a duplicate, after asking.

**Pull requests** in the sidebar lists the open ones. Click a PR to see the full diff in the app,
with **Open in browser** and **Merge** (merge commit, squash and merge, or rebase and merge, with
a confirmation). After a merge the app fetches right away, so the merge commit shows up in the
graph without a Fetch by hand.

**Right-click a branch -> Create pull request…** opens the create dialog: source and target
repository (so a fork works too), the branches, a title and a description, or **Write title and
description with Claude** to have Claude write them from the commits and the diff. Afterwards
you get the URL of the new pull request.

To have a PR reviewed: the **Review with Claude** button on its row in the sidebar, or on the
pull request itself, types the review instruction into a Claude session. **PR review** in the top
row opens a session of its own for any pull request URL you paste into it.

## Terminals and Claude

- **Terminal** (with a +) opens a shell (PowerShell on Windows), **Claude** (with a +) opens one
  that starts `claude` straight away. Both open in the repository you have open. The button is the
  last one on the right of the view row; the sessions are tabs in the row under it, and the close
  button on a tab ends that session.
- Terminals keep running in the background - on another repo tab, in another view. Only closing
  the tab itself ends a session.
- **What Claude is doing** shows as a dot after the title of its tab: coral and pulsing while
  Claude works, yellow while it waits for your answer (a permission question, say), and green for
  an answer that finished while you were looking elsewhere - until you look at that tab. Hover the
  tab to read it in words. When Claude waits for you or finishes while the window is not in front,
  or while you are in another tab, view or repository, Windows shows a notification; click it to
  go straight to that session. A session that was already running before you updated the app
  shows its dot from the next session on.
- **Selected text is copied to the clipboard immediately**, so selecting is all it takes.
  Because of that **Ctrl+C is the interrupt again**, even with text selected. **Ctrl+Shift+C**
  copies explicitly, **Ctrl+V** and **Ctrl+Shift+V** paste, and right-click pastes when nothing
  is selected.
- A quick click never selects: the pointer has to be held down and dragged a little before a
  selection starts, so a click that lands next to the input box does not overwrite the clipboard.
- **Prompt history:** the **Prompt history** button (Claude view) slides open a column with
  everything you asked that session, numbered and timed, with **Reuse** and **Copy** per line.
  Claude's answers bury your questions otherwise. After a `/resume` the prompts of the session you
  picked up appear above the ones you gave here, under a **Resumed here** line.
- **Artifacts:** the **Artifacts** button (Claude view), next to **Skills**, lists the artifacts Claude published on claude.ai from a Claude Code session
  in this repository or one of its worktrees, newest first, with their count on the button. Click
  one to open it: in the Claude desktop app when that is installed, in your browser otherwise.
  With the desktop app installed, **Open in browser** at the end of a row opens it in the browser
  instead.
  The list is read from Claude Code's own session history on this computer, so an artifact made
  from claude.ai in the browser or on another computer does not appear.
- **Skills:** the **Skills** button (Claude view), next to **Artifacts**, lists the Claude
  Code skills a session here can use: your personal ones (every project), the ones of this
  repository (`.claude/skills`, checked in with it), and read-only the ones from your claude.ai
  account and from installed plugins. Claude picks a skill by itself when a request matches its
  description; **Use** at the end of a row types its command into the session to run it
  explicitly. Click a skill to open it, or **New skill…** at the bottom to write one. The editor
  shows the whole `SKILL.md`; on the right, describe what the skill should do (or what should
  change) and click **Ask Claude**: Claude writes or rewrites the skill and puts it in the editor,
  with **Undo** if you prefer the old version. Left empty, it sharpens the description and
  tightens the steps of the current skill. Nothing is saved until you click **Save**. **Delete**
  moves the skill's folder to the recycle bin. A claude.ai or plugin skill is replaced on its next
  update, so it cannot be edited here; **Copy as personal skill** gives you a copy that can.
- **Claude usage:** once a Claude session in the app has had its first answer, the status bar at
  the bottom shows how much of your plan is used, in every view: **Current session** (the 5-hour
  window) and **Weekly limit** side by side, each with a bar, the percentage used and its reset
  time. The bar turns orange from 70% and red from 90%. Hover it to see how old the
  numbers are. It only updates while a Claude session in the app is working; in between it shows
  the last known numbers, faded after half an hour. Claude Pro and Max only.
- **Model and effort:** left of the usage, in the Claude view, the status bar shows the
  model of the session on screen and its effort (and **Fast mode** when it is on). A `/model` or `/effort` switch shows up
  with Claude's next answer.
- **Drag a file onto a terminal tab** (a screenshot, say) and its path lands on the prompt, ready
  for you to type a question after it.

## Checklist and client presentation

When Claude has worked on something, the **Checklist** menu (Claude view) turns what that
session changed into one of two things. Both are written by the session on screen, which knows
what it did. Switch the menu on with **Checklist and client presentation** under
**Settings -> Claude**.

- **Checklist:** Claude publishes an artifact with every change of the session, grouped by subject
  and numbered A1, A2, B1 and so on, each with a checkbox and a line on what to check and where.
  Tick the points off as you go; the artifact keeps them ticked.
- **Presentation for the client:** fill in the client's name, who it is addressed to (optional) and
  the language, then **Make the presentation**. The session lists its changes and writes the
  slides, in plain words without code or jargon, and the app takes screenshots of the pages where
  the changes can be seen. The slides are in the website's house style: the logo, support colour
  and font are looked up in the project, and the logo and colour can be changed in the dialog.
  - **Edit text and screenshots** lets you correct the text per slide before it goes out, and add,
    replace or mark the screenshot of each change. **Add screenshot** captures a page with the Edge
    or Chrome on your computer (desktop 1440x900 and/or mobile 390x844 - pick the defaults in
    Settings), or takes an image you paste (**Ctrl+V**) or drop. The capture only opens the page;
    it cannot log in or click.
  - **Marking a screenshot:** click a thumbnail to open it full size and draw a **Circle**, a
    **Box**, an **Arrow** or a **Number** on it. **Select** a mark and press **Delete** to remove
    it. Marks Claude suggests are dashed until you click **Use Claude's marks**.
  - **Save as PDF…** saves the slides as a PDF where you pick, one slide per page, ready to send.
    **Open PDF** opens it.
- **Where it is kept:** in the repository (or worktree) the session works in, under
  `.claude/checklists/`. That folder ignores itself, so nothing of it ever shows up in **git
  status** or a commit, and your own `.gitignore` is not touched.

Your company name and logo are set once under **Settings -> Claude**, **Presentations are
prepared by**. They are on the cover and on the last slide, where the logo is shown in white on a
dark background - so a logo in one colour (an SVG is best) works well.

## Keyboard shortcuts

| | |
|---|---|
| **Ctrl+1 … Ctrl+6** | Git, Claude, Explorer, Terminal, Issues, Worktrees |
| **Ctrl+`** | Jump to the Terminal view, and back to where you were |
| **Ctrl+F** | Search the commits (Git view) |
| **Ctrl+P** | Command palette: fetch, pull, push, stash, check out any branch, switch repo tab, open a worktree, settings, a new terminal |
| **Ctrl+C** | Interrupt what the terminal is running |
| **Ctrl+Shift+C** | Copy the terminal selection (selecting already copies it) |
| **Ctrl+V** | Paste into the terminal, or a copied file into the Explorer tree |
| **Ctrl+S** | Save the file you are editing in the Explorer now |
| **Left Shift twice**, **Ctrl+Shift+N** | Go to any file in the project by name; twice again includes ignored files and dependencies (Explorer) |
| **Right Shift twice**, **Ctrl+Shift+F** | Search file names and the contents of every file (Explorer) |
| **Ctrl+E** | Recently used files (Explorer) |
| **Ctrl+G** | Go to a line in the open file (Explorer) |
| **Ctrl+B** | Go to where the name under the caret is defined (Explorer) |
| **Ctrl+F**, **F3**, **Shift+F3** | Find in the open file, next and previous match (Explorer) |
| **Ctrl+R** | Find and replace in the open file (Explorer) |
| **Ctrl+/** | Comment the selected lines out or back in (Explorer) |
| **Ctrl+D** | Duplicate the line or the selection (Explorer) |
| **Alt+Shift+Up/Down** | Move the selected lines up or down (Explorer) |
| **Ctrl+W**, **Ctrl+F4** | Close the file on screen (Explorer) |
| **Ctrl+Tab**, **Alt+Left/Right** | Next or previous open file (Explorer) |
| **Ctrl+Shift+V** | Switch a Markdown file between its rendered and its code view (Explorer) |
| **Up / Down**, **Home / End**, **Page Up / Page Down** | Walk the commits in the graph (with **Shift**: select a range), the files of a commit or of your changes (the diff follows), the branches in the sidebar, the file tree, issues, worktrees and search results |
| **Left / Right** | Close or open a folder in the file tree or the sidebar |
| **Enter** | Do what a click on the row does |
| **Menu key**, **Shift+F10** | The right-click menu of the row the keyboard is on |
| **Esc** | Close a menu, cancel a dialog (it asks first when you typed something), clear a search field, or clear the selection in the graph |

The shortcuts work while you are typing in a terminal; the shell does not see them.

## Settings

Every option explains itself behind a **?** icon next to it or next to its section title: hover
it to read the explanation, or click it to keep it open.

- **Workspace** - identities and their GitHub/Bitbucket accounts, and the SSH key per provider.
- **Claude** - **Use Claude** on or off, the model and effort a new Claude session starts
  with, the model and effort for resolving conflicts, the model that writes commit messages, the
  command a new Claude session runs, and
  the check that tells you whether Claude is usable. **Claude Code default** leaves the choice to
  your own Claude Code settings. **Checklist and client presentation** switches the Checklist menu
  on, with the default screenshot sizes and the name and logo your presentations are signed with.
- **Repository** - commit signing (GPG or SSH, with **Test signing** that signs a throwaway
  object), the external editor, the default worktree folder, what gets copied into a new
  worktree, the browser preview and the local site server. The first time a new external editor
  is used, the app asks once whether to start that program.
- **General** - theme (System, Dark or Light, applied instantly, terminals included; a new Claude
  session also gets the light or dark version of your Claude Code theme; a session that was already
  running keeps its colours, and a banner names those tabs so you can close and reopen them), which views
  and columns are shown, the start view, font sizes per part of the app, and **Export**/**Import**
  of all your settings as one JSON file. **Toolbars, tabs & buttons** also sets the size of every
  button in the app, so they are the same size everywhere.
- **About** - version, update check, and a link back to this manual.

## When something goes wrong

**"git was not found on PATH"** - install git, then restart GitWorkflow so it picks up the new
`PATH`.

**A Claude button does nothing, or Claude is greyed out** - it is almost always one of four: `claude`
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

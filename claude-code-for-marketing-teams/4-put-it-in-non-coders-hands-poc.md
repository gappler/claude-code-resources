---
title: Putting it in non-coders' hands — the surfaces beyond the terminal
description: A proof-of-concept walkthrough of the Claude Code surfaces a non-technical marketer actually touches — the desktop app and the browser/mobile web version — with an honest read on which one needs a code repository and which is the real on-ramp.
date: 2026-08-12
version: 1.2
status: published
audience: marketing teams evaluating Claude Code
source: Claude Code docs — claude-code-on-the-web.md, web-quickstart.md, desktop-quickstart.md
---

# Putting it in non-coders' hands

Everything in this series so far might have read like it lives in a terminal, a command line, a developer's tool. It doesn't have to. Claude Code runs the same engine behind a few different surfaces, and one of them is a normal desktop app with buttons and panes. For a marketing team the surface isn't a footnote. It's the thing that decides whether people actually use any of this. A capability nobody will open is worth nothing.

> [!TIP]
> **The point:** The desktop app's Local mode is the real on-ramp. It's graphical, needs no terminal, and works against a plain folder without a code repository.

The frame comes straight from the Claude Code docs. **Claude Code behaves the same everywhere.** What changes between surfaces is where the work runs and whether it needs a code repository. That second part matters most for a non-technical user, and it's where the easy assumption ("just use the no-terminal version") goes wrong. So this piece is mostly about picking the right door in.

---

## 1. The surfaces, compared

There are three ways a person interacts with Claude Code. Two of them need no terminal:

| | **Desktop app** | **On the web (browser + phone)** | **Terminal (CLI)** |
| :--- | :--- | :--- | :--- |
| What it is | A normal app with panes and buttons | Runs in a browser or the mobile app | The command line |
| Where work runs | Your machine | Anthropic's cloud | Your machine |
| Needs a code repository? | **No** | **Yes**, it clones a GitHub repo | No |
| Needs a terminal? | No | No | Yes |
| Best for | The non-technical on-ramp | Repo-based work, monitored from anywhere | Developers |

The headline for a marketing team is simple. **The desktop app in "Local" mode is the real on-ramp.** It has a graphical interface, needs no terminal, and works against a plain folder on your machine without requiring a code repository. The web version is genuinely terminal-free too, but it's built around a GitHub repo, a meaningful catch we'll come back to.

(All of these need a paid Claude subscription, whether Pro, Max, Team, or Enterprise. There's no free tier for Claude Code.)

---

## 2. The desktop app, the most accessible door

### What it is

The Claude Code desktop app is a standard Mac or Windows application. A sidebar, a chat pane, a visual view of changes, an optional file editor and terminal you can ignore. No command line required. When you start a session you choose where it runs, and **Local** runs it on your own machine against a folder you pick.

### Why a marketer cares

This is the version you'd actually put in front of a non-technical person:

- **It works with your real files.** Point it at a content folder, a notes directory, a drafts folder. No repository, no git, no setup ceremony. It reads and edits what's there.
- **You see and approve every change.** By default it runs in **Manual** mode (older builds labeled this "Ask permissions"). Claude proposes a change, shows you a visual before and after, and waits for you to accept or reject. Nothing touches your files until you say yes. For someone nervous about handing work to an AI, approving each step is the thing that builds trust. It isn't a black box.
- **Everything from the earlier pieces works here.** Skills, connectors, and your CLAUDE.md brand rules all function in the desktop app the same way, since it shares configuration with the other surfaces. So `/copy-qa`, your brand voice rules, and your connected tools come along.

### How to use it

Download the app, sign in, open the **Code** tab, choose **Local**, pick a folder, and type what you want, like "review the drafts in this folder against our style guide." You review each proposed change and accept it. That's the whole loop. Skills are a `/` away, and files come in by dragging them into the prompt or typing `@filename`.

### Honest constraints

The desktop app runs on Mac and Windows, with a Linux build now in beta. For a non-technical marketer that's rarely the deciding factor, since they're almost always on a Mac or Windows machine anyway.

> [!WARNING]
> **The honest read:** The desktop app has three tabs, and only one of them is Claude Code. Alongside the <strong>Code</strong> tab there's a <strong>Chat</strong> tab (a plain chat, like the claude.ai app) and a <strong>Cowork</strong> tab (a separate autonomous agent that runs on its own). Cowork is not Claude Code, and it sits outside this series. When you onboard someone, point them at the Code tab specifically, or they'll land somewhere that doesn't do what these pieces describe.

**A personal note.** The desktop app is the on-ramp I'd give a first-timer. My own daily setup is Claude Code running as an extension inside an editor like VS Code. You install it from the editor's extensions panel, point it at any folder with no repository required, and get inline diffs, @-mentions, and plan review in the window you already work in. The same Skills, brand rules, and connectors come along, since the local surfaces share one configuration. It's a step up from the on-ramp rather than the first door, and with a little setup help it tends to click quickly.

---

## 3. Claude Code on the web, powerful but repo-bound

### What it is

Claude Code on the web runs in your browser (and the mobile app) at a web address, with the work happening on Anthropic's cloud rather than your machine. You submit a task, Claude works on it in the cloud, and you review the result from your laptop or your phone, since sessions persist across devices. It's in research preview, so it's still evolving, worth knowing before a client builds a workflow on it.

### Why a marketer cares

The appeal is real. No install, no terminal, and you can kick off work from a browser and check it from your phone later. You review changes visually, leave comments on specific lines, and the session keeps running even if you close the tab.

### The honest catch

> [!WARNING]
> **The honest read:** The web version requires a GitHub repository. It clones a repo into a cloud machine to work in, then pushes its results back as a branch. That's natural for software teams and a genuine obstacle for a marketing team whose content lives in folders, a CMS, or a drive rather than a code repository.

What that means for a non-technical marketer depends on where the content already lives:

- If the client's content already lives in a repo (some publishing setups do), the web version is a clean, no-terminal way to work with it from anywhere.
- If it doesn't, which is the common case, the web version's value comes through **connectors** and MCP tools wired in at the repo or environment level (the CMS, drive, and analytics from piece two) rather than local files — set up once by someone technical, since the desktop app's point-and-click connector menu isn't available in cloud sessions.
- For a marketer working with files on their own machine, the **desktop app's Local mode** is the better fit, full stop.

---

## 4. How it fits together: onboarding a non-technical marketer

The realistic path to getting someone productive without a terminal:

1. **Start in the desktop app, Code tab, Local mode.** Install, sign in, point it at a folder of real work. No repo, no git, no command line.
2. **Leave it in Manual mode.** Every change is proposed and shown before it's applied, and the person approves each one. Trust builds because nothing happens behind their back.
3. **Give them one or two Skills.** A `/copy-qa` or `/campaign-brief` from the first piece turns the open-ended tool into a few clear buttons they can press.
4. **Let the brand rules ride along.** The CLAUDE.md voice and vocabulary rules apply automatically, so their output sounds right without them prompting for it.
5. **Add the browser and mobile version later, if it fits.** Once they're comfortable, and if the content lives somewhere a repo or connector can reach, the web version lets them kick off and check work from anywhere.

The order matters. Start where the friction is lowest (desktop, local, approve-each-change), then expand. Lead with the cloud or web version and you lead with the GitHub requirement, which is exactly the wall a non-technical user hits first.

---

## What to take away

"Put it in non-coders' hands" has a clear best answer: the **desktop app in Local mode.** It's graphical, needs no terminal, works with real files and folders without a repository, and its approve-each-change default is the trust mechanism that gets a nervous first-time user comfortable. Everything from the earlier pieces (Skills, connectors, brand rules) comes along.

The **web and mobile** version is genuinely terminal-free and great for working from anywhere, but it's built around a GitHub repository, so it fits repo-based or connector-driven work more than a marketer with files on their desktop. "No terminal" never means "no subscription," and it only means "no repository" on the desktop's Local mode.

Pick the surface to match the person and where their work lives. Next, the governance questions a client asks before any of this gets the green light, and last, rolling it out to a team.

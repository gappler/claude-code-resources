---
title: Connecting AI to the tools clients already use
description: A practical walkthrough of how Claude Code connects to the systems a marketing team already runs on — CMS, analytics, drives, project tools, CRM — through MCP, plus an honest read on triggering from Slack and chat, and the controls that answer "where does our data go?"
date: 2026-08-12
version: 1.2
status: published
audience: marketing teams evaluating Claude Code
source: Claude Code docs — mcp.md, mcp-quickstart.md, managed-mcp.md, slack.md, channels.md, third-party-integrations.md
---

# Connecting AI to the tools clients already use

The first piece in this series was about turning a marketing process into a reusable system. This one is about where that system gets its information and where it does its work. An AI that can only see what you paste into a chat box is a smarter notepad. An AI that can read your content library, pull last month's analytics, see the brief in your project tool, and check the brand assets in your drive is part of how the work actually gets done.

The mechanism that makes this possible is **MCP**, the Model Context Protocol. It's the single most important idea in this piece, so most of what follows is about MCP and what it opens up. The rest is a plain accounting of two things people assume work better than they currently do (triggering from Slack and chat), and the controls that answer the question every client asks before connecting anything. Where does our data go, and who controls it?

> [!TIP]
> **The point:** An AI wired into the tools you already run on (CMS, analytics, drive, project tool), instead of export, paste, prompt.

---

## 1. MCP, the connective tissue

### What it is

MCP is an open standard for connecting an AI to outside tools and data. In plain terms, an **MCP server** is a small connector that exposes one system (a CMS, a database, a project tracker, a drive) to Claude, so Claude can read from it and act on it directly. Claude Code can connect to a large and growing catalog of them.

The signal that you need one is simple, and it comes straight from the Claude Code docs. **Connect a server when you find yourself copying data into chat from another tool.** If someone is pasting analytics exports, campaign briefs, or competitor notes into the conversation by hand, that's a connector waiting to be set up. Once it's connected, Claude reads and acts on the live system instead of working from a stale paste.

### Why a marketer cares

This is what moves AI from a side tool to part of the operating system. With the right connectors in place, the work stops being "export, paste, prompt" and starts being "ask." A few examples, all using connectors that exist today:

- **Content and docs.** Notion, Google Drive, and similar. "Pull the brand guidelines from the drive and check this draft against them." "Find every blog post we published last quarter about pricing."
- **Project and task tools.** Asana, Linear, issue trackers. "Read the brief in this task and draft the three social variations it asks for."
- **Analytics and data.** A database or analytics connector. "What were our top five performing posts last month by engagement?" "Find the segments that haven't opened an email in 90 days."
- **CRM and marketing platforms.** HubSpot and others expose MCP servers. "Summarize the open opportunities tagged for the spring campaign."
- **Design.** Figma and similar tools expose MCP servers too, so a mockup can be pulled straight into a task.

The point isn't any single connector. It's that the brief, the data, the assets, and the brand rules can all be in the room when Claude works, instead of something a person has to fetch and paste first.

### How to use it

You don't build these connectors yourself. For most marketing tools, someone has already published an MCP server, and you browse the reviewed ones in Anthropic's connector directory. Connecting one is simple. Add it by name, sign in through your browser if it needs authentication, and it's available. The docs' own first-server walkthrough is three steps (add, check it connected, use it). How you add a connector looks a little different depending on whether you're in the terminal, desktop app, or browser, and piece four covers the surface a non-technical marketer actually uses.

Two things worth understanding up front:

- **Authentication is normal and handled for you.** Most cloud tools use a browser sign-in (OAuth). You add the connector, run `/mcp`, and approve it in the browser the same way you'd connect any app to any other app. Tokens are stored securely and refreshed automatically.
- **A connector can be personal or shared.** You can add one just for yourself, or a team can commit a shared configuration to the project so everyone gets the same set of tools automatically. The recommended pattern for a team is exactly that. One person sets up the connectors once, and the whole team inherits them.

Two smaller capabilities make connectors feel native once they're set up:

- **Reference data with `@`.** Type `@` and you can pull a specific resource from a connected tool straight into your prompt (a particular doc, issue, or record) the same way you'd reference a file.
- **Connector commands.** Some servers expose ready-made actions that show up in the `/` menu, so a common task against a connected tool becomes a one-word command.

One caution from the docs. **Only connect servers you trust.** A connector that pulls in outside content is a path through which bad instructions could reach the AI, so the connectors you wire in should be ones you or your client vetted. Governance, later in this piece, is the other half of this.

---

## 2. Triggering from where the team works

The appealing version is non-technical marketers firing off AI tasks from Slack or their phone, where they already work. That's a real direction, but it's worth being precise about what works today versus what's emerging, because the gap matters for what you'd promise a client.

### Slack, real but coding-shaped today

There's a Claude integration for Slack where mentioning `@Claude` with a task spins up a session. The coding path is the built-out one: it detects coding intent, routes to Claude Code on the web (the browser-based version, not the claude.ai chat app), works against your connected GitHub repositories, and opens its work as a pull request, pulling context from the Slack thread. That's powerful for a product or engineering team reacting to bug reports in a channel. Non-coding messages can route to plain Claude Chat instead, but that's a general assistant, not your Claude Code setup. One caveat on plan tiers: on Team and Enterprise this per-user Slack version is being retired in favor of **Claude Tag**, where `@Claude` runs as the organization's shared identity with admin-configured access — while the per-user path above remains on Pro and Max.

> [!WARNING]
> **The honest read:** For a marketing team, this is not yet a clean "trigger a content workflow from Slack" path. You can chat with Claude there, but the route wired into Claude Code, with your Skills and brand rules, is the coding one, which assumes a GitHub repo. The direction exists and is improving, but I wouldn't build a client promise on marketers running content jobs from Slack today.

### Channels, early and technical

There's a newer capability called **channels** that lets outside events (a chat message from Telegram, Discord, or iMessage, or a webhook from another system) push into a running Claude session so it can react while you're away. The interesting marketing-adjacent use is the "chat bridge." Message Claude from your phone and get the answer back in the same chat while the work runs against your real files.

The caveats. Channels are **in research preview**, the setup is **technical** (creating bot tokens, installing plugins, running from a terminal), and a session has to be actively running to receive events. This is promising for a hands-on operator, not something to put in front of a non-technical marketer yet.

### The durable takeaway

The reliable connective story today is **MCP**, wiring Claude into the data and tools the work depends on. Triggering from chat surfaces is real and moving fast, but coding-oriented and early. If a client's question is "can our team kick off content jobs from Slack," the honest answer is "the pieces are forming, but the solid version right now is connecting Claude to your tools, not chatting at it from Slack."

A note on automation pipelines. If a client wants AI steps inside an automated publishing or CI pipeline (triggered by events rather than a person), that's the subject of the next piece in this series, running things on a schedule and hands-off, where GitHub Actions and scheduled runs fit. It's a real path. It just belongs with automation, not with "connect to tools."

---

## 3. Governance, where the data goes and who controls it

This is the question that comes before any connection in an agency or enterprise conversation, so it's worth treating as part of the connectivity story rather than an afterthought.

The reassuring fact for clients is that **an administrator can control exactly which connectors are allowed**. This isn't all-or-nothing. The controls, in plain terms:

- **A fixed, approved set.** An organization can deploy a specific list of connectors, and users get exactly those and can't add others. Everyone works against the same vetted tools.
- **An allowlist or a denylist.** Permit only approved connectors and block everything else, or block known-bad ones and allow the rest.
- **Off entirely.** An organization can disable outside connectors completely if policy requires it.
- **Per-user credentials.** Connectors can authenticate each person through their own sign-in, so each user only reaches what they personally have access to, rather than everyone sharing one key to a client's systems.

For a client conversation, the useful framing is that connecting AI to your tools is **a governed decision, not a free-for-all.** A central team decides what's connected, individuals authenticate as themselves, and an admin can see and restrict what's in use. That's the answer to "is our brand and customer data going to leak through some connector we didn't approve." The controls to prevent that are built in. (The deeper questions, whether our content is used for training and how long it's kept, are their own topic, covered in the governance piece later in this series.)

---

## 4. How it fits together, a connected content workflow

Put the connectors in place and a real task stops being a scavenger hunt:

1. The **brief** lives in the team's project tool, so Claude reads it directly from the task instead of someone pasting it.
2. The **brand guidelines and assets** are in a connected drive, so Claude checks the draft against the live guidelines, not a copy from six months ago.
3. **Last month's performance** comes from a connected analytics or data source, so "lead with the angle that performed best" is a question Claude can actually answer.
4. The finished pieces get written back to the **content or CMS tool** through its connector, a write-capable connection, which (unlike the read-only pulls above) you'd gate with approvals. More on that in the governance piece.
5. The whole connector set was configured once by one person and shared, so every marketer on the team works against the same tools without setting anything up.

No exporting, no pasting, no stale copies. The information the work depends on is in the room.

---

## What to take away

What MCP changes is simple: the brief, the data, the assets, and the brand rules are all in the room when Claude works. It's how Claude stops being a clever chat window and becomes something wired into the CMS, the analytics, the drive, and the project tool a marketing team already runs on — a governed connection an administrator controls, not an open door.

The chat-trigger story (Slack, phone, webhooks) is real and improving, but it's coding-shaped and early today, so it's worth knowing about without overpromising. And the trust question that gates all of it has a clear answer. Connections are vetted, authenticated per person, and centrally controllable.

If the first piece was "turn a process into a system," this one is "connect that system to the real work." Next, running it on a schedule, with no one at the keyboard.

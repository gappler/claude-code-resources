---
title: Building reusable marketing workflows with Claude Code
description: A practical walkthrough of the Claude Code building blocks a marketing team uses to turn repeatable processes into AI systems — Skills, subagents and agent teams, dynamic workflows, and output styles.
date: 2026-08-12
version: 1.2
status: published
audience: marketing teams evaluating Claude Code
source: Claude Code docs — skills.md, agent-sdk__skills.md, sub-agents.md, agents.md, agent-teams.md, workflows.md, common-workflows.md, output-styles.md, memory.md
---

# Building reusable marketing workflows with Claude Code

Most of what's written about Claude Code treats it as a coding tool. For a marketing team, that framing buries the part that matters. Underneath the code-specific features is a general system for building and governing AI agents that get work done and stay inside the guardrails you set. That's the part marketing should care about, because the work a marketing team repeats is exactly the kind of process this system can capture and run. This piece walks through the building blocks you'd assemble to turn a marketing process into something anyone on the team can run the same way every time.

> [!TIP]
> **The point:** Turn a process you repeat into a system the whole team runs the same way, every time.

---

## 1. The mental model: a process, not a prompt

A prompt is something you type once and lose. A process is something you want to do the same way every time, but until it is automated, it drifts depending on who's running it.

Claude Code gives you three ways to automate a process:

| Tool | What it holds |
| :--- | :--- |
| **CLAUDE.md and memory** | The standing facts and rules that should be true in every session. |
| **Skills** | A repeatable procedure, such as building a brief or QA-ing copy. |
| **Subagents, agent teams, and workflows** | A big job split across workers running at once. |

You don't need all three to start. You need to know which one fits the thing you're trying to make repeatable.

One more building block sits alongside these, but in a different category. An output style isn't a way to automate a process; it's a personal voice default you layer on top of these tools for a session dedicated to writing. Section 5 covers it.

---

## 2. CLAUDE.md and Skills: capture a repeatable process

Two of the three automation tools are just Markdown files you write once. They differ in when they load. CLAUDE.md loads every session and holds the rules that should always apply. A Skill loads only when you invoke it and holds a procedure you run on demand.

### CLAUDE.md, the standing rules

`CLAUDE.md` is a plain Markdown file Claude reads at the start of every session. It's where you write down what you'd otherwise re-explain every time: who the audience is, how your team names and files things, which tool to reach for when, and the standing always-do and never-do rules, brand vocabulary among them.

**Why it matters.** This is how a team's conventions stop depending on whoever is at the keyboard. You write a rule once and it loads into every session automatically. The rules stay short and blunt, like "the audience is non-technical marketers," "drafts live in `/drafts`," or "never use 'unlock,' 'leverage,' or 'empower.'" When Claude makes the same mistake twice, or a review catches something it should have known, that's the signal to add a line.

**How to use it.** Keep it short, under about 200 lines, because it loads into every session and longer files get followed less reliably. Be specific and checkable: "lead with the reader benefit in the first sentence" beats "write good copy." There's also an **auto memory** feature, where Claude notes your corrections and preferences over time and builds up your own working style without you writing it all down. It's personal, so rules the whole team shares still belong in the committed CLAUDE.md.

**Where it lives.** Like a Skill, a CLAUDE.md comes in levels, and they stack:

| Where it lives | Who gets it |
| :--- | :--- |
| `~/.claude/CLAUDE.md` (personal) | You, across all your own projects. Travels to no one. |
| `CLAUDE.md` at the repo root | The whole team, committed with the project. |
| `CLAUDE.md` in a subfolder | The team too, and it loads only when Claude is working in that folder. |

Every level that applies loads at once, so your personal rules and the project's stack together. What reaches the team is the committed files, the repo-root and any folder-level ones; your personal `~/.claude/CLAUDE.md` stays yours.

The folder-level file is the one worth knowing about. A team running several channels can put channel-specific rules in a `CLAUDE.md` inside each channel's folder, so the social voice rules load when Claude works in the social folder and the email rules when it's in email. That's how a brand's voice bends by channel without one giant rulebook.

> [!WARNING]
> **The honest read:** CLAUDE.md is context, not an enforced rule. Claude reads it and tries to follow it, but it's guidance, not a hard gate. For something that has to happen every time, like a check that runs before a piece ships, you'd use a hook: a command Claude Code runs automatically on every action, regardless of what Claude decides. Hooks are an advanced topic this piece doesn't cover, but the enforcement option is there for when a rule truly can't be optional.

### Skills, what they are

The docs give a blunt, useful rule of thumb: create a Skill when you keep pasting the same instructions, checklist, or multi-step procedure into chat. If you've explained "here's how we build a campaign brief" three times this month, that explanation should be a Skill.

A Skill is a folder with a `SKILL.md` file inside it. That file has a short description at the top and a set of instructions below. Once it exists, Claude treats it as part of its toolkit. It runs the Skill when your request matches the description, or you trigger it by name with `/skill-name`.

The efficiency that matters: a Skill's instructions load only when the Skill runs. You can keep a long, detailed procedure in a Skill and it costs almost nothing until someone invokes it. Standing rules that should always apply belong in CLAUDE.md (above). Procedures that run on demand belong in Skills.

### Why a marketer cares

This is the unit that turns a process into a system. Concrete examples:

- **A brief builder.** `/campaign-brief` walks through your standard intake (objective, audience, channels, success metrics, deadlines) and produces a brief in your house format every time, instead of a different shape depending on who wrote it.
- **A copy QA checklist.** `/copy-qa` runs the same pass on every piece: claims substantiated, CTA present, reading level, banned phrases, legal flags. The checklist is identical whether it's a junior or a director running it.
- **A performance report.** `/campaign-recap` takes a campaign's results and works them through the same set of questions every time (what moved, against which goal, what to change next) so every report has the same shape instead of depending on who wrote it.

The instructions can include a template for Claude to fill in, an example of the expected output, and even small scripts the Skill runs. So a brief Skill can carry your actual brief template, and a reporting Skill can generate a formatted file rather than just text in a chat window.

### How to use it

You make a directory and write one Markdown file. Where you put the directory decides who can use the Skill:

| Where it lives | Who gets it |
| :--- | :--- |
| `~/.claude/skills/<name>/` | You, across every project |
| `.claude/skills/<name>/` (in a project) | Anyone working in that project |
| Shared via the team's repo | The whole team, version-controlled |

The `description` field is the most important line you'll write. It's how Claude decides when to reach for the Skill on its own, so it should include the words people would naturally say ("brief," "QA this copy," "break down this campaign"). Put the main use case first.

Two controls worth knowing from the start:

- For a Skill with real consequences (anything that publishes, sends, or commits), set `disable-model-invocation: true` so it runs only when a person types the command. You don't want Claude deciding on its own that the copy is ready to ship.
- Keep the instructions tight. Once a Skill loads, it stays in the conversation, so every line is a recurring cost. State what to do, not why.

---

## 3. Subagents, agent teams, and workflows: scale the work across many workers

A single assistant is one writer. A content operation is a pipeline (someone drafts, someone edits, someone fact-checks, someone formats). Claude Code has three ways to run that pipeline, and they differ mainly in who holds the plan.

Here's the decision in one line each:

- **Subagents.** Claude delegates side tasks to helpers inside one conversation and collects their results. Lightweight, the default choice.
- **Agent teams.** Multiple independent sessions that talk to each other and share a task list, coordinated by a lead. For work that needs discussion.
- **Dynamic workflows.** A script that runs many agents at once and cross-checks their results. For jobs too big to coordinate by hand.

### Subagents, the workhorse

A subagent is a specialized helper that runs in its own separate context, does one focused task, and returns only a summary. The verbose middle (the files it read, the searches it ran) stays out of your main conversation.

**Why marketing cares.** You can define a small roster of specialists and reuse them:

- A **copy-editor** agent that only reviews and suggests, with no ability to overwrite the original.
- A **fact-checker** agent restricted to read-and-research, so it can verify claims but not change copy.
- A **brand-compliance reviewer** that checks a draft against your guidelines and flags violations by severity.

Two patterns map directly onto content production:

- **Run research in parallel.** "Research these three competitors at the same time using separate subagents." Each works independently, then the results come back together.
- **Chain them.** "Use the copy-editor to find weak spots, then the rewrite agent to fix them." Each hands its output to the next.

**How to use it.** Ask Claude to set one up for you, or write a small Markdown file yourself with a name, a description (Claude uses this to decide when to delegate), and the instructions that define the specialist. The two controls that matter most for marketing:

- **Limit the tools.** A reviewer should be read-only. Give it the ability to read and search, but not to write or edit. This is how you stop a "reviewer" from rewriting the thing it's supposed to review.
- **Give it a focused job.** One subagent, one thing it's excellent at. A vague all-purpose agent performs worse than a sharp narrow one.

Subagents can also keep a persistent memory across sessions, so a brand-compliance reviewer can accumulate "here are the mistakes this team keeps making" over time and get better at catching them.

### Agent teams, when the workers need to talk

Subagents report back to the main conversation and never talk to each other. An agent team is different. Several full Claude sessions work in parallel, share a task list, and message each other directly, with one session acting as lead.

**Why marketing cares.** The standout use case is review from several angles at once. A single reviewer drifts toward one kind of problem. Split the review into independent lenses and each gets full attention at the same time. Three reviewers on one piece (one on brand voice, one on factual accuracy, one on legal and claims), and the lead synthesizes their findings.

The "competing hypotheses" pattern also translates. If a campaign underperformed and nobody's sure why, you can have several agents each investigate a different theory and argue it out, which surfaces a better answer than one analyst settling on the first plausible story.

**How to use it.** Agent teams are experimental and off by default. You turn them on with a setting, then describe the team in plain language ("create a team to review this launch from three angles"). Start with 3–5 teammates, and start with research or review tasks rather than parallel writing, since two agents editing the same file overwrite each other.

> [!WARNING]
> **The honest read:** Agent teams use significantly more tokens than a single session, because each teammate is a full instance of Claude. That's worth it for research and review, and overkill for routine work.

### Dynamic workflows, when the job is too big to coordinate by hand

A workflow is a script that orchestrates many agents and, critically, can have them check each other's work before anything is reported. Claude writes the script from your description. It runs in the background while you keep working, and you get one result at the end.

**Why marketing cares.** This is the tool for scale and trust:

- A **content audit across an entire site**, every page checked against a fixed rubric, far more than one conversation could hold.
- **Cross-checked research**, a competitive or market question where you want claims verified against multiple sources, with the unsupported ones filtered out before you ever see them.
- A **review you run on every piece**, drafted once, saved, and re-run identically each time.

The built-in `/deep-research` workflow is the easiest way to see this: give it a question and it fans out searches across several angles, cross-checks the sources, and returns a cited report with the claims that didn't hold up already removed.

**How to use it.** Type your request and either ask for "a workflow" in plain words or include the keyword `ultracode`. Claude shows you the plan before it runs. Once a run does what you wanted, you save it with a keystroke and it becomes a `/command` you can run forever. Two things to keep in mind. A single run uses meaningfully more tokens (test on a small slice first, one section before the whole site), and you can watch progress and stop it at any point, usually without losing finished work.

### Which one to reach for

| You want… | Use |
| :--- | :--- |
| A focused helper to keep your main chat clean | **Subagent** |
| Reviewers who compare notes and challenge each other | **Agent team** |
| A repeatable, large-scale job with built-in cross-checking | **Workflow** |

Default to subagents. Move up only when the work genuinely needs it.

---

## 4. Everyday patterns and custom commands

The orchestration tools above are the heavy machinery. Most days, the value comes from a few smaller, concrete patterns and from turning your favorites into one-word commands.

### Patterns worth lifting directly

- **Work in any folder, not just code.** Claude Code runs inside a notes folder, a docs directory, or any pile of Markdown the same way it runs in a codebase, searching, editing, and reorganizing content. Your content library is a valid workspace.
- **Reference files and assets with `@`.** Point Claude at a specific brief, brand doc, or folder with `@filename` and its full content is in the conversation instantly, no hunting.
- **Work from images.** Drag in a screenshot, a design mockup, or a competitor's ad and ask Claude to analyze it or describe what it shows. Useful for design feedback and visual teardowns.
- **Plan before acting.** For anything you want to review before it changes, plan mode has Claude propose what it'll do and wait for your approval before touching anything.
- **Resume across sittings.** A campaign plan can span days. You can reopen a session and keep going, but on long work Claude compacts the older parts of the conversation, so a clean handoff sometimes beats a raw resume.
- **Delegate research to keep your context clean.** "Use a subagent to research X" sends the messy exploration elsewhere and brings back only the findings.

### Custom commands

A custom slash command is just a Skill you trigger by name, the same mechanism from section 2. The practical move is to notice the things you do repeatedly and give each one a command:

| Command | What it does |
| :--- | :--- |
| `/campaign-brief` | Your standard intake-to-brief process |
| `/copy-qa` | The fixed quality pass on any draft |
| `/repurpose` | Turn one asset into channel-specific variations |
| `/weekly-digest` | The recurring report in its standard shape |

These show up in the `/` menu for the whole team and run identically every time. That consistency, not raw capability, is what makes the difference between a tool a few power users like and a system a team actually relies on.

---

## 5. Output styles: a voice default for a writing session

An output style is for one situation in particular: a session where Claude is acting as your brand copywriter throughout. There, it pins the voice and format once, so you're not re-prompting "make it sound like us" every turn.

What makes it a whole-session tool, and not a switch you flip for a single deliverable, is how it works. An output style is session-wide, and it changes how Claude responds to everything, its role, tone, and format, by editing the system prompt directly. That includes how Claude talks to you, not just the copy it produces.

**How to use it.** It's a Markdown file of instructions, selected through `/config`. You choose whether to keep Claude's built-in software-engineering behavior or drop it for a pure writing assistant. It holds on every response until you switch it.

> [!WARNING]
> **The honest read:** Because it reshapes every response, including how Claude talks to you, an output style fits a session where Claude is your writer throughout, not something you flip on for one branded asset. And by default it's a personal setting. For a brand voice a whole team relies on, the layers that load and travel automatically are CLAUDE.md, for the rules and vocabulary, and shared Skills. Treat an output style as a personal default layered on top of those, not the thing that carries the brand.

### Which layer holds what

| The thing you're pinning | Put it in |
| :--- | :--- |
| Facts and rules (vocabulary, audience, "always/never") | **CLAUDE.md** |
| A repeatable procedure that runs on demand | **Skill** |
| The default voice and format for a writing session | **Output style** |

---

## 6. How it fits together: a content-production pipeline

Put the blocks in one line and the system becomes concrete. Imagine producing a launch's worth of content:

1. **CLAUDE.md** carries the brand's vocabulary, audience, and banned phrases, loaded into every session before anyone types a word.
2. `/campaign-brief` (a **Skill**) turns the kickoff notes into a structured brief in your house format.
3. A **workflow** or a set of **subagents** drafts the channel variations in parallel (blog, email, social), each from the same brief.
4. A **copy-editor subagent** and a **brand-compliance reviewer** (read-only, so they can't overwrite anything) pass over every draft.
5. `/copy-qa` (a **Skill**) runs the final fixed checklist before anything goes out.

If the whole session is spent writing, a personal output style can pin the voice for that session.

No step depends on remembering how it's done, and no step changes shape based on who ran it. That's the whole idea: the same marketing process, captured once, run the same way by anyone, every time.

---

## What to take away

The reason this is worth a marketing team's attention isn't that an AI can write copy, plenty of tools do that. It's that Claude Code lets you capture a process and govern it: package the steps as a Skill, split the work across reviewers and writers, and pin the brand's rules and voice so they hold without re-prompting.

CLAUDE.md, Skills, and subagents are the trio that turns "we use AI for marketing" into "we built our marketing process into a system." Commit them to the project and all three travel to the whole team. An output style is a personal default you add on top for a dedicated writing session. Start with the trio. The rest is detail.

Next, connecting that system to the tools your team already runs on.

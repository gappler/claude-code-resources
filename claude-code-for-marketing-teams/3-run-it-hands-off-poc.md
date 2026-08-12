---
title: Running it hands-off — scheduled and automated work
description: A proof-of-concept walkthrough of how Claude Code runs marketing work on a schedule or in response to events with no one at the keyboard — cloud routines, local desktop tasks, in-session loops, and pipeline automation — plus an honest read on what assumes a code repository and what doesn't.
date: 2026-08-12
version: 1.2
status: published
audience: marketing teams evaluating Claude Code
source: Claude Code docs — routines.md, desktop-scheduled-tasks.md, scheduled-tasks.md, headless.md, github-actions.md
---

# Running it hands-off

The first two pieces were about work you start. You package a process, connect it to your tools, then run it. This one is about work that runs itself: a weekly content audit done before you open your laptop, a competitor digest waiting in Slack every Monday, a monthly report that assembles itself. This is the part a client feels week after week, which makes it the retention story. Recurring value that shows up without anyone asking for it.

"Hands-off" means no one is at the keyboard when the work runs. That changes how you set it up. The instructions have to be self-contained, and you decide in advance what the AI is allowed to touch. The back half of this piece is about doing that well, because it's where unattended automation succeeds or quietly goes wrong.

> [!TIP]
> **The point:** Recurring marketing work that runs on a schedule without anyone driving it. This is the retention story.

One thing shapes everything below, so it's worth saying first. **Most of Claude Code's hands-off automation is built around a code repository.** The cloud option clones a GitHub repo to work in, and the pipeline options live inside GitHub. One exception runs against a plain folder on your machine, and for a marketing team that exception is often the most practical starting point. I'll flag which is which as we go.

(This is Claude Code, the agentic tool, not the claude.ai chat app. Some of these features are configured through a web page, but the thing running is Claude Code.)

---

## 1. Three ways to schedule, and how they differ

Claude Code has three ways to run work on a schedule. They differ along one line worth understanding before you pick, local versus cloud.

| | **Cloud (routines)** | **Desktop (local tasks)** | **In-session (`/loop`)** |
| :--- | :--- | :--- | :--- |
| Runs on | Anthropic's cloud | Your machine | Your machine |
| Needs your computer on | No | Yes | Yes |
| Needs a session open | No | No | Yes |
| Sees your local files | No (works from a fresh repo clone) | Yes | Yes |
| Best for | Reliable recurring jobs that run without you | Work that needs your actual files and tools | Quick polling while you're working |

In plain terms:

- **Cloud routines** are the "set it and forget it" option. They run even when your laptop is closed. The catch is that they work inside a cloned GitHub repository, not your local files.
- **Desktop tasks** run on your machine through the Claude Code desktop app, against a folder you choose, which can be a content or notes folder with no repo required. The catch is that your computer has to be awake.
- **`/loop`** keeps re-running a prompt while a session is open. It's for watching something finish, not for unattended weekly work. It's the most technical and least relevant of the three for marketing, so I'll set it aside.

For a marketing team, the realistic choice is usually **cloud routines** (for reliability) or **desktop tasks** (for working with your real files without a repo). The rest of this piece focuses on those two.

---

## 2. Cloud routines: recurring work that runs without you

### What it is

A routine is a saved Claude Code setup (a prompt, the place it works, and the connectors it can use) that runs automatically on Anthropic's cloud infrastructure. Because it runs in the cloud, it keeps working when your computer is off. You create and manage routines from a web page or the CLI, and each run shows up as a session you can open and review.

### Why a marketer cares

This is the recurring-value engine, the kind of work that's valuable precisely because it happens on a rhythm:

- **A weekly competitor digest.** Pulls from your sources, summarizes what changed, posts it to Slack every Monday morning.
- **A scheduled content audit.** Scans a body of content against a fixed rubric on a cadence and flags what's drifted or underperformed.
- **A monthly performance summary.** Assembles the numbers and the narrative into a report on the first of the month, without anyone building it by hand.

The Claude Code docs' own examples are dev-flavored (backlog grooming, PR review), but the shape is identical. The work is unattended, repeatable, and tied to a clear outcome.

### How to use it

You give the routine a name, write the prompt it runs every time, pick where it works, choose which connectors it can use, and set a trigger (a schedule like "every weekday at 9am," and/or the event triggers in section 4). Then it runs on its own.

### The caveats

Routines are powerful but come with real conditions, straight from the docs:

- **They're in research preview.** Behavior and limits may shift. Fine for internal use and demos, but be measured about putting a client's critical process on one today.
- **They run fully autonomously.** No approval prompts mid-run, and an included connector can write without asking. So you scope tightly, giving it only the connectors the job needs and a network setting that reaches only what's required.
- **Connectors come from your claude.ai account,** not from servers you added locally in the terminal (this ties back to piece two, the connectors a routine can use are the ones on your account).
- **Runs draw down your usage,** and there's a daily cap on how many can start. Recurring value has a recurring cost.

> [!WARNING]
> **The honest read:** A routine clones a GitHub repository at the start of every run. For dev work that's natural. For marketing content it means the content either lives in a repo, or the routine works entirely through connectors (your CMS, drive, analytics) rather than files. This is the single biggest "is this practical for us" question, so scope it honestly before you put it in front of a client.

---

## 3. Desktop tasks: recurring work against your real files

### What it is

A desktop scheduled task starts a fresh Claude Code session automatically, at a time you choose, on your own machine through the desktop app. Unlike a cloud routine, it has **direct access to your local files and tools** and doesn't require a code repository. You point it at a folder.

### Why a marketer cares

This is the more accessible on-ramp for a content team, because it sidesteps the repo requirement. If your content, briefs, and notes live in folders on your machine, a desktop task can work with them directly:

- **A morning briefing** that pulls from your calendar and inbox (via connectors) and lands a summary before you start the day.
- **A recurring audit** of a local content folder against your standards.
- **A weekly cleanup or reorganization** pass over a working folder.

It's also closer to where a non-technical marketer already works, the desktop app rather than a terminal.

### How to use it

In the desktop app, you set up a scheduled task by naming it, writing the instructions the same way you'd type a prompt, picking the folder it works in, setting its permission mode, and choosing a schedule (hourly, daily, weekdays, weekly, or a custom time you describe in plain language). When it fires, you get a notification and a new session you can open.

### The caveats

- **Your computer has to be awake.** If it sleeps through the scheduled time, the run is skipped (the app does one catch-up run on wake, for the most recent miss). Even with a keep-awake setting, closing the lid usually still sleeps the machine. For true always-on, that's what cloud routines are for.
- **A run might fire late** (e.g., on wake), so if timing matters, build guardrails into the prompt itself, like "only look at today's items; if it's after 5pm, just summarize what was missed."
- **Permissions can stall a run.** If the task needs a tool it isn't pre-approved for, it waits for you. The fix is to run it once manually, approve what it needs, and let future runs auto-approve.

---

## 4. Triggering on events, not just the clock

Beyond a schedule, a cloud routine can start in response to **events**, which is how "hands-off" becomes "reactive." Two trigger types matter:

- **An API trigger** gives the routine a web address another system can call. When something happens in a tool you use, that tool can fire the routine and pass along context. For example, a form submission or an alert kicks off a follow-up automatically.
- **A GitHub trigger** runs the routine when something happens in a repository, useful if a client's publishing pipeline is git-based (e.g., a routine that runs whenever new content is merged).

Both sit at the more technical end. The API trigger assumes someone can wire up an authenticated web call, and the GitHub trigger assumes a repository. Real and powerful for a team with that plumbing, more than a non-technical marketer sets up alone. The capability is worth knowing about. Scope the setup realistically.

---

## 5. AI steps inside a pipeline: the developer-facing end

Two more capabilities round out the picture, both genuinely code-oriented. I'm including them for completeness and to be clear about where the line is:

- **Headless mode** (`claude -p`) runs Claude Code as a command-line step, no interactive session, just a prompt in and a result out. It's the building block for scripts and automated pipelines: an automated brand-and-style check that runs on a schedule, a batch job that processes many files. It's developer-facing; a marketer wouldn't run it directly, but it's what powers the more polished automations.
- **GitHub Actions** lets Claude Code run automatically inside a GitHub repository's pipeline, on a schedule or in response to repo events. If a client's content lives in GitHub and they want AI steps in that pipeline, this is the path.

The reality is that both assume a code repository and someone comfortable with that setup. They're the right answer when a client already has a git-based publishing pipeline and a developer to wire it up, not a starting point for a content team on their own.

---

## 6. The real skill: writing prompts for unattended runs

This is the part that determines whether hands-off automation helps or quietly produces garbage, and it's worth its own section because the docs are emphatic about it. Picture the Monday competitor digest that posts an empty summary every week — because the prompt never told it which sources to read, or where a "nothing changed" result should go. That's the failure this section exists to prevent.

When no one is at the keyboard, **the prompt can't ask a clarifying question.** So it has to be self-contained and explicit. The discipline:

- **Define what success looks like, in the prompt.** Not "review the content" but "check each page against these five criteria, flag any that fail, and post the list to the #content channel."
- **Say what to do with the result.** Where does the output go? A Slack message, a draft, a file, a report? An unattended run that does good work and leaves it nowhere useful is a wasted run.
- **Build in guardrails for timing,** since a run may fire late, like "only today's items; if it's after hours, just summarize."
- **Scope what it can touch.** The right connectors and nothing more, the right folder or repo and nothing more. Autonomy plus broad access is how unattended runs cause damage.

> [!WARNING]
> **The honest read:** "It ran" isn't "it worked." A green checkmark means the run finished, not that it did the right thing. Open the early runs and confirm what actually happened before you trust the rhythm, and keep spot-checking once you do.

Here's a good way to frame it for a client. An unattended automation is only as good as the instructions you'd give a competent assistant who can't call you to ask. Write it that way.

---

## 7. How it fits together: a weekly content operation

Put it on a rhythm and the recurring work runs itself:

1. **Monday, 7am.** A routine (or desktop task) compiles last week's content performance and a competitor scan, and posts the digest to a Slack channel through a connector. The team starts the week briefed, with no one having assembled it.
2. **Wednesday.** A scheduled audit passes over the content library against the brand and quality rubric (the `/copy-qa` checklist from piece one), and flags anything that's drifted.
3. **First of the month.** A routine assembles the monthly report from connected analytics and lands a draft for review.
4. **On demand.** When a specific event fires (a campaign launches, a form comes in), an API trigger kicks off the matching follow-up.

Every one of these is the same shape: a self-contained prompt, scoped access, a clear destination for the output, and a human reviewing the early runs. That's recurring value the client feels, the work that's done before they think to ask for it.

---

## What to take away

Hands-off automation is the retention story, recurring value that shows up on a schedule without anyone driving it. The map for a marketing team is two real options and a caveat.

- **Cloud routines** for reliability. They run without your machine, but they work inside a GitHub repo or through connectors, and they're still early and changing.
- **Desktop tasks** for working with your real files. No repo needed, the most practical starting point, but your computer has to be awake.
- **The pipeline options** (headless, GitHub Actions) are powerful and genuinely code-bound. They're right when a client already has a git-based pipeline and someone to wire it up, not before.

The skill that makes any of it work is writing the prompt as if for an assistant who can't ask you a question: explicit about success, scoped in what it can touch, clear about where the output goes, and reviewed until it's trusted.

That's the series spine complete. You package a process, connect it to the tools, and run it on its own. The remaining pieces cover the surfaces a non-technical marketer actually touches, the governance questions a client asks before any of this gets the green light, and how it all rolls out to a team.

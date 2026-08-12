---
title: Governance and trust — the questions a client asks before buying in
description: A proof-of-concept walkthrough of the three pre-purchase questions every marketing client raises about Claude Code — where does our data go, what can the agent actually do, and what will it cost — with honest answers and the plan-by-plan nuances that matter.
date: 2026-08-12
version: 1.2
status: published
audience: marketing teams evaluating Claude Code
source: Claude Code docs — data-usage.md, zero-data-retention.md, legal-and-compliance.md, permissions.md, sandboxing.md, security.md, costs.md, monitoring-usage.md
---

# Governance and trust

Every piece before this one was about what Claude Code can do. This one is about the conversation that happens before a client lets it do any of it. For an agency or an in-house team, the build doesn't start until three questions get honest answers:

1. **Where does our data go?** Is our brand work, our content, our customer information safe?
2. **What can the agent actually do?** Can we keep it from going off the rails?
3. **What will it cost, and can we prove it was worth it?**

These aren't objections to overcome with confidence. They're reasonable, and the answers are mostly good. The catch is that each one carries real nuances that depend on which plan you're on. Getting those nuances right is the difference between a trust-building conversation and a promise that falls apart under procurement's questions.

> [!TIP]
> **The point:** Clients ask three things before buying in, data, control, and cost. The honest answers are mostly good.

(This is Claude Code, the agentic tool, not the claude.ai chat app. The data and trust rules differ between the two, which matters below.)

---

## 1. Where does our data go?

This is the first question, and the headline answer is reassuring, with a plan-dependent asterisk you have to get right.

### The training question

What clients fear most is that their brand work gets used to train an AI their competitors then use. The answer depends on the plan:

- **On commercial plans (Team, Enterprise, and the API), Anthropic does not train its models on your prompts or your work.** This is the clean, strong answer for any business client, and it's the default, not something you switch on.
- **On consumer plans (Free, Pro, Max), training is a choice you make.** Anthropic uses your data to improve models when that setting is on. An individual marketer on a personal Pro plan should check it. A client on a Team or Enterprise plan doesn't have to.

The practical guidance is simple. If data sensitivity matters at all, the client should be on a **Team or Enterprise plan**, where non-training is the standing default.

### How long data is kept

- **Commercial plans keep data 30 days** by default.
- **Zero Data Retention (ZDR)** is available for qualified Enterprise accounts. Prompts and responses aren't stored at all once the response comes back.
- **A local footnote worth knowing.** Claude Code stores session transcripts in plain text on the user's own machine (about 30 days by default) so sessions can resume. That's local, not on Anthropic's servers, but it's a real data-handling fact for anything sensitive.

> [!WARNING]
> **The honest read:** ZDR is not part of the standard Enterprise plan. Anthropic's account team has to request and enable it after an eligibility check, so you can't toggle it on yourself. Don't tell a client they "have ZDR" until you've confirmed it's actually been turned on. It's the easiest thing in this whole conversation to over-promise.

### Security and compliance posture

The credible, procurement-facing facts:

- Data is **encrypted in transit** (TLS) and **at rest**.
- Anthropic maintains **SOC 2 Type 2** and **ISO 27001**, with the reports available through its Trust Center, the artifacts a security team will ask for.
- For healthcare, a **BAA (HIPAA) can extend to Claude Code**, but only with ZDR enabled. HIPAA-grade handling is possible, gated on the ZDR step above.

### The limits

Three things to state plainly rather than gloss:

- **ZDR doesn't cover everything.** Even when it's enabled, it doesn't extend to the claude.ai chat app, the Cowork agent, or data handled by **third-party integrations and connectors** (the MCP tools from piece two). Those follow their own providers' policies, so review them separately.
- **Consumer plans train by choice, not never.** Don't describe Pro/Max as "your data is never used." It's "you control whether it's used," which is a different sentence.
- **Connectors send data to other services.** The moment you connect a CMS or analytics tool, that tool's data handling is in scope too. Anthropic's policies cover Anthropic, they don't cover the connector.

---

## 2. What can the agent actually do?

The second fear is an autonomous agent doing something unintended, deleting the wrong thing, publishing before it's ready, touching files it shouldn't. The reassuring truth is that Claude Code is **permission-gated by default**, and the controls are real and enforceable.

### The default is cautious

Out of the box, Claude Code is **read-only**. It can look, but any action with consequences (editing a file, running a command) requires your explicit approval. You approve once, or choose "always" for things you trust. And it can only write inside the folder where it was started, not your wider system. For a nervous first-time client, this approve-each-step default (the same one from the non-coders piece) is the core of the trust story. Nothing happens behind their back.

### You can scope it tightly

Beyond the default, you can define exactly what an agent is allowed to do:

- **Allow, ask, and deny rules** spell out which tools and commands are permitted, which always prompt, and which are forbidden outright. A "deny" is a hard block.
- These rules can be **checked into a shared project and enforced for everyone**, and an organization's admin can deploy **managed settings that individual users can't override**. That's the answer to "can we set guardrails the whole team has to follow." Yes.
- This is how the read-only reviewer agents from the first piece actually stay read-only. You deny their ability to write, and the client enforces it, not the model's good intentions.

### A sandbox for stronger isolation

For more autonomous use, Claude Code can run commands inside a **sandbox**, operating-system-level limits on which files and which internet domains a command can touch, enforced by the OS regardless of what the agent decides to do. It's the difference between "we asked it not to" and "it can't." In practice: an agent running your weekly analytics pull can be boxed in so it reaches your analytics tool and nothing else — not the rest of your machine, not the open web.

### The distinctions that matter

This is where precision earns trust, because the controls are not all equally hard. The line that matters most runs between what's enforced and what's only guidance.

> [!WARNING]
> **The honest read:** Permission rules and managed settings are genuinely enforced by Claude Code, independent of the model's judgment, so a deny rule blocks the action. CLAUDE.md and prompt instructions are not. "We told it in CLAUDE.md not to touch X" shapes behavior but doesn't guarantee it, so it isn't a control. For a real boundary, use a deny rule or the sandbox. Conflating the two is the most common way a trust claim falls apart.

The rest of the distinctions still matter.

- **The sandbox reduces risk, it isn't airtight.** The docs are explicit. It doesn't inspect encrypted traffic, broad domain allowances can become data-exfiltration paths, and it doesn't run on native Windows (Mac/Linux/WSL2 only). Sell it as strong risk reduction, not a perfect wall.
- **Secrets can be locked down explicitly.** A sandbox credentials setting denies reads of files like `~/.aws/credentials` and `~/.ssh` and strips or masks secret environment variables from sandboxed commands, so the agent can't read them while it works. It's a first-party control for exactly the worry a client raises.
- **Connectors aren't security-audited by Anthropic.** Anthropic reviews directory listings but doesn't vet what a given MCP server does. Connector trust is the user's responsibility, which is why an admin can restrict exactly which ones connect (piece two).
- **Ultimately, you're responsible for review.** The docs say it directly. Claude Code has only the permissions you grant, and you review what it proposes. The right framing for a client isn't "it's perfectly safe," it's "you decide what it can do, and the limits you set are enforced."

---

## 3. What does it cost, and can we see it?

The third question is budget and ROI. Claude Code is priced on usage (tokens), so cost is real and variable, but it's both **capped** and **measurable**, which is what a client actually needs to hear.

### What it costs

The realistic framing. The published benchmark, roughly **$13 per active day and $150–250 per month per user**, comes from enterprise deployments, where usage is heavy. A marketing team's usage pattern is different and usually lighter, so treat that figure as a ceiling reference from a heavy-use context, not a quote for a content team. The real answer for any client is to run a small pilot and measure, which the tools below make straightforward.

### You can cap it

- On the API, an admin can set **workspace spend limits**. On Pro/Max, there's a **monthly spend limit** on usage credits. Cost isn't open-ended unless you leave it that way.
- The expensive features are predictable. **Agent teams and large workflows multiply token use** (each agent does its own full read of the material, so more agents means proportionally more spend). Those are opt-in and scopeable, not silent background spend.

### You can see it

- **`/usage`** shows token use and an estimated cost for an individual, broken down by what consumed it (skills, subagents, connectors).
- For a team, Claude Code can **export usage and cost data** (via OpenTelemetry) into a monitoring dashboard, so an organization can track spend **per user and per team**, and Team and Enterprise accounts get an **analytics dashboard**. That's the raw material for showing a client ROI per engagement, what was run, how much it cost, what it produced.

### What to watch

- **Team-level cost monitoring is an enterprise, technical setup.** The per-person `/usage` view is simple. Wiring usage data into a company dashboard is an IT project. Don't imply per-client ROI dashboards are a one-click feature.
- **On some cloud deployments (Bedrock, Google Cloud, Foundry), Anthropic doesn't see the cost metrics.** Tracking there relies on the cloud provider's own tooling. It matters only if a client insists on those platforms.
- **Cost scales with how you use it.** Model choice (a lighter model for routine work), tight prompts, and good context habits materially lower spend. ROI is partly a function of using it well, not just a fixed price.

---

## 4. The pre-buy checklist

What you'd actually confirm before telling a client "yes, this clears your requirements":

1. **Plan.** Team or Enterprise if data sensitivity matters at all, that's where non-training is the default. Don't run sensitive client work on a personal Pro plan without checking the training setting.
2. **ZDR, if required.** If the client needs zero retention or HIPAA, confirm ZDR has actually been requested and enabled by the account team. It's not automatic.
3. **Guardrails.** Decide what agents may and may not do, write it as enforced permission rules (and managed settings if it must hold team-wide), and lean on read-only defaults for review work.
4. **Connectors.** Vet each connected tool's own data handling, and use admin controls to restrict which connectors are allowed. Anthropic's policies don't extend to them.
5. **Cost.** Run a small pilot, watch `/usage`, set a spend cap, and establish a real baseline before quoting the client a number.
6. **Compliance artifacts.** Pull the SOC 2 / ISO reports from the Trust Center for the client's security team.

Do that, and the governance conversation stops being a hurdle and becomes a reason to trust the engagement.

---

## What to take away

The three questions, data, control, and cost, all have genuinely good answers, and the honest version is more persuasive than the rosy one:

- **Data.** On Team and Enterprise plans your work isn't used for training, it's encrypted, and the compliance certifications exist. The asterisks are the consumer-plan training choice and the fact that ZDR is a separate, requested step.
- **Control.** The agent is permission-gated by default, and you can hard-enforce the limits an admin sets. CLAUDE.md is guidance, real boundaries are deny rules and the sandbox, and the sandbox is strong but not airtight.
- **Cost.** It's usage-based, cappable, and measurable. The published figures are an engineering benchmark, so pilot and measure for a marketing team.

That's the conversation that clears the work for launch. A process packaged as a system, connected to the client's tools, run on its own, put in non-technical hands, and cleared through the governance conversation. The throughline across these pieces is that the honest version, with its limits stated, is the one that earns the engagement. The last piece is rolling it out to a team.

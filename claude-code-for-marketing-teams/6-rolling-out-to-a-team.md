---
title: Rolling it out to a team — Claude Code for Teams and Enterprise
description: A practical walkthrough of how an individual's Claude Code usage carries over to a marketing team or agency on the Teams and Enterprise plans — what replicates identically, and the org layer that gets added around it (shared config, enforced guardrails, access, and visibility).
date: 2026-08-12
version: 1.2
status: published
audience: marketing teams and agencies evaluating Claude Code at team scale
source: Claude Code docs — admin-setup.md, server-managed-settings.md, analytics.md, authentication.md, third-party-integrations.md, plus team-relevant sections of permissions.md, data-usage.md, costs.md, monitoring-usage.md
---

# Rolling it out to a team

Every piece in this series so far has described one person using Claude Code. The natural next question, the one a marketing lead or agency owner asks once they're convinced, is "how does this work when it's not just me?" When eight marketers need it, or an agency wants to run it across several client engagements, what changes?

The short answer is reassuring. Your individual usage replicates exactly. Teams and Enterprise don't give you a different Claude Code; they wrap the same tool in a layer that makes it shareable, governable, and visible across a group. Nothing you learned in the other pieces stops working. It just becomes something a whole team inherits instead of something each person sets up alone.

This piece is about that layer, kept deliberately at a high level rather than down in the admin weeds (no SSO-provisioning or device-management detail). For the data, control, and cost specifics, it leans on the governance piece rather than repeating them.

> [!TIP]
> **The point:** Your individual usage replicates on a team plan. What you gain is shared config, enforced guardrails, access, and visibility.

---

## 1. Does my usage replicate? (Yes, here's the nuance)

Start with the question you'd ask yourself first. Everything you've done as an individual carries over to a team plan unchanged:

- **Skills, subagents, workflows, output styles, CLAUDE.md** are all identical. The Claude Code docs are explicit that Claude Code behaves the same everywhere; the engine doesn't change by plan.
- **Connectors (MCP)** work the same.
- **The surfaces** (desktop, web, terminal) are the same.

One thing is worth knowing. A few features require a Claude.ai-based plan (Team, Enterprise, or a personal Pro/Max), including Claude Code on the web, routines, and remote control. If an organization deploys Claude Code through a cloud provider instead (the Bedrock, Google Cloud, or Foundry route this series otherwise skips), those particular features need separate Team or Enterprise seats. For a marketing team, that's an argument for staying on the straightforward Team/Enterprise path rather than a cloud-provider deployment.

So your usage doesn't get worse on a team plan. It gets **shared, governed, and measured**, the three things the rest of this piece covers.

---

## 2. The plans, briefly

Four ways to run Claude Code at an organization, but for a marketing team it's really two:

| Plan | What it is | Best for |
| :--- | :--- | :--- |
| **Claude for Teams** | Self-service, per-seat. Collaboration, admin tools, centralized billing | Most marketing teams and small agencies |
| **Claude for Enterprise** | Adds SSO, role-based permissions, compliance API, and ZDR eligibility | Larger orgs with security/compliance requirements |
| Claude Console | API-first, pay-as-you-go | API-centric setups |
| Cloud providers (Bedrock/Google Cloud/Foundry) | Inherit existing AWS/GCP/Azure billing and compliance | Orgs already standardized on a cloud |

The recommendation from the docs is the simple one. For most organizations it's Teams or Enterprise: one per-seat subscription, Claude Code and claude.ai together, no infrastructure to run. Team is self-service; Enterprise is sales-led and adds the heavier governance (SSO, compliance API, ZDR). Pick Enterprise when a client's security team is in the room. Team is plenty for a marketing group getting started.

---

## 3. Access: who gets it

On a team plan, people get in through **seats**. An admin invites team members, assigns seats, and those members log in with their Claude.ai accounts. Enterprise adds **SSO and domain capture**, so access flows through the company's existing identity system rather than individual invites.

One practical note for rollout. A seat has to actually include Claude Code access. If someone sees "you haven't been added to your organization yet," their seat doesn't cover Claude Code and an admin needs to update it. Worth knowing so onboarding doesn't stall on day one.

This is the "us, not me" enablement, but the real multiplier is the next section.

---

## 4. Shared config: the actual team value

This is the part that makes a team plan worth it, and it ties directly back to the first piece. Everything you built as an individual (your Skills, your brand-voice rules, your connectors) can be distributed so the whole team inherits it automatically:

- **Skills and brand rules travel.** The `/campaign-brief` and `/copy-qa` Skills and the CLAUDE.md voice-and-vocabulary rules from piece one can be committed to a shared project or packaged as a plugin, so every marketer on the team gets the same repeatable processes and the same brand enforcement without setting anything up. Build the system once; the team runs it.
- **Connectors can be shared** so everyone reaches the same CMS, drive, and analytics (piece two).
- **A Team or Enterprise admin can push org-wide instructions** (a managed CLAUDE.md) that load in every session and can't be skipped, useful for a standing brand or compliance reminder across the whole org.

This is the difference between "a few power users have clever setups" and "the team has a shared operating system." For an agency, it's also how you'd standardize an approach across client engagements: a base set of Skills and rules everyone starts from.

---

## 5. Enforced guardrails: governance at team scale

The governance piece covered which controls exist (permission rules, sandbox, data settings). The team layer is about enforcing them across everyone, so they don't depend on each person configuring things correctly.

- **Managed settings** let an admin define policy (which tools and commands are allowed, which connectors can connect, what's denied) that individual users can't override. This is the hard-enforced layer from the governance piece, applied org-wide.
- **Two ways to deliver it.** Enterprise and Team admins can push settings **from the Claude.ai admin console**. This is server-managed, so no device-management software is needed and the settings arrive when people sign in. Organizations with device management (MDM) can instead deploy settings **to the machines directly**, which is harder for a user to tamper with.
- **An admin can turn whole surfaces on or off** org-wide: web sessions, routines, remote control, channels. This is the bridge to the surfaces question, the team layer decides which doors are even open, on top of how each individual uses them.
- **An admin can also govern which models the team uses.** Enterprise settings can restrict which models are available, set the default, and cap how much reasoning effort agents spend. For a marketing team that's a single lever on both consistency and spend, decided once for everyone rather than left to each person.

> [!WARNING]
> **The honest read:** Managed settings are a client-side control, not a security boundary. On an unmanaged device a user can work around them without needing admin or sudo access, for instance by running a modified or older client that never applies the policy. For genuine enforcement, device-managed (MDM) settings on company-controlled machines are stronger. For most marketing teams this is academic, but it's the accurate version if a security team asks "can someone just turn this off?"

---

## 6. Visibility: usage and cost, with a catch for marketing

Team plans add dashboards. Here's where the marketing-versus-engineering distinction matters most, so I'll be direct about it.

**What's genuinely useful for a marketing team:**

- **Adoption and usage metrics** such as daily active users, sessions, and who's actually using it. Good for spotting who needs help and whether the rollout is landing.
- **Cost visibility** through `/usage` per person, and (with the OpenTelemetry setup from the governance piece) per-user and per-team spend in a dashboard. This is your raw material for tracking what an engagement costs.

The part that probably won't fit is the dashboard's headline numbers.

> [!WARNING]
> **The honest read:** The headline "ROI" and "contribution" metrics in the analytics dashboard are built for engineering. They count pull requests and lines of code shipped with Claude Code, attributed via GitHub. A marketing team mostly doesn't produce PRs and lines of code, so the leaderboard and contribution charts will look empty or irrelevant. Don't promise a client a "Claude Code ROI dashboard" and then show them a tool that measures code commits. For marketing, ROI is better told through adoption, cost data, and the marketing output itself — campaigns shipped, briefs turned around, drafts through QA — not the built-in contribution metrics. Those metrics also aren't available under ZDR at all, another reason not to lean on them for a security-conscious client.

---

## 7. How it fits together: an agency rolling out across a team

Concretely, standing this up for a marketing team or across an agency's clients:

1. **Pick the plan.** Team for a marketing group getting started; Enterprise when a client's security and compliance requirements are in scope (SSO, ZDR, org-level model governance).
2. **Assign seats** and confirm they include Claude Code access. SSO on Enterprise if the org already uses it.
3. **Distribute the system.** Commit the shared Skills (`/campaign-brief`, `/copy-qa`) and brand-voice CLAUDE.md so every member inherits them. This is the payoff, the brand-consistency system from piece one, running team-wide.
4. **Set the guardrails once.** Managed settings define what agents can and can't do across everyone; turn off any surfaces the team shouldn't use yet.
5. **Connect shared tools** and restrict which connectors are allowed (piece two).
6. **Watch adoption and cost**, not the code-contribution dashboard. Pilot small, establish a baseline, set a spend cap.

The result is not eight people each rigging up their own setup, but one governed, shared system: your marketing process, distributed and enforced, with spend you can see.

---

## What to take away

Teams and Enterprise don't change Claude Code. They wrap your individual usage in four things: shared config (your Skills and brand rules, inherited by everyone, the real reason to go team), enforced guardrails (managed settings an admin sets and users can't override), access (seats and SSO), and visibility (adoption and cost data).

Two notes carry the most weight. Pick Team for simplicity, Enterprise when compliance is in the room. And the built-in ROI dashboard is engineering-shaped, so a marketing team should measure adoption and cost, not lines of code.

Your usage replicates. What you gain is the ability to make it the whole team's.

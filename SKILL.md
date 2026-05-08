---
name: craft
description: "Design judgment for AI-assisted work. Reviews UI/UX, triages quality, and defends against overbuilding through ten sub-commands. Reads PRODUCT.md, DESIGN.md, and FEATURES.md (with Operating Conditions: consequence, environment, user skill level) to calibrate output to actual stakes. Use when the user invokes /craft or $craft, asks to get started, review a design for failure modes, scope, hierarchy, affordance, responsiveness, or quality, or wants to set up design context. Triggers on /craft start, /craft document, /craft features, /craft coach, /craft risk, /craft signal, /craft compose, /craft responsive, /craft scope, /craft ship, \"design review,\" \"what to cut before launch,\" \"is this ready to ship,\" \"does this work on mobile,\" \"users don't know what to click,\" and similar UI/UX evaluation requests."
---

# /craft

Design judgment for AI-assisted work. Frameworks for evaluating UI/UX, triaging quality, and defending against overbuilding.

## What /craft is

Ten coordinated sub-commands organized into three categories:

- **Foundation** — writes the context files that every other command reads: PRODUCT.md (strategy and Operating Conditions), DESIGN.md (visual system), FEATURES.md (what the product does).
- **Judgment** — reviews UI through specific lenses: coach (pedagogical critique), risk (failure modes), signal (affordance), compose (composition fundamentals), responsive (cross-context behavior).
- **Ship** — restraint commands: scope (defend against overbuilding), ship (triage what's load-bearing).

Where most design tools focus on aesthetics, /craft is about judgment under stakes — design that has to hold up under real user mistakes, real environments, real consequences.

## Sub-command routing

When the user invokes any sub-command, load the corresponding reference file and follow its instructions:

| User invocation | Load |
|----------------|------|
| `/craft start` | references/start.md |
| `/craft brief` | references/start.md |
| `/craft document` | references/document.md |
| `/craft features` | references/features.md |
| `/craft coach` | references/coach.md |
| `/craft risk` | references/risk.md |
| `/craft signal` | references/signal.md |
| `/craft compose` | references/compose.md |
| `/craft responsive` | references/responsive.md |
| `/craft scope` | references/scope.md |
| `/craft ship` | references/ship.md |

Each reference file is a complete instruction set for the sub-command. Read it, then execute its workflow.

When the user invokes bare `/craft`, bare `$craft`, or says only that craft is loaded, route to `references/start.md`. Do not ask what lens to run. Start by scanning the project and giving one clear next step.

## Auto-routing for freeform requests

When the user runs `/craft <freeform request>` without a specific sub-command, pattern-match the request and pick the right reference file:

| Request language | Routes to |
|------------------|-----------|
| "set up," "start here," "new project" | start.md → document.md → features.md |
| "explain," "why is this," "review for understanding" | coach.md |
| "what fails," "failure modes," "what could go wrong," "edge cases" | risk.md |
| "users don't know what to click," "affordance," "discoverability," "is this clickable" | signal.md |
| "hierarchy is off," "grouping," "Gestalt," "composition," "structural" | compose.md |
| "mobile," "responsive," "across devices," "touch," "small screen" | responsive.md |
| "we're overbuilding," "cut features," "too many options," "too complex" | scope.md |
| "ready to ship," "load-bearing," "what to defer," "polish triage" | ship.md |
| "document features," "update features.md," "what does this do" | features.md |
| "build me X," "make a Y," "redo this Z" | freeform build (load PRODUCT.md and DESIGN.md, build, optionally suggest review pass) |

When the request is ambiguous, surface the two or three plausible commands and ask the user to pick. Do not silently route to a default.

## The foundation files

Every /craft command reads three files at the project root before doing work:

- **PRODUCT.md** — strategic context. Register, users, purpose, voice, references, anti-references, and Operating Conditions (consequence when wrong, environment, user skill level). Written by /craft start.
- **DESIGN.md** — visual system in Google Stitch format. Six fixed sections (Overview, Colors, Typography, Elevation, Components, Do's and Don'ts). Written by /craft document.
- **FEATURES.md** — canonical feature inventory at five description levels (one-liner, short, marketing, technical, status). Written and reconciled by /craft features.

Without these files, every command falls back to generic SaaS defaults and produces output uncalibrated to the actual product. The single most important thing /craft does is enforce that calibration.

If any foundation file is missing when a command runs, offer to invoke the relevant Foundation command before proceeding.

## Operating Conditions: the wedge

PRODUCT.md carries three fields most design tools never ask about:

- **Consequence when wrong** — reversible inconvenience / lost work / real-world harm / regulatory exposure
- **Environment** — where, when, how the product is used (desk + focused, mobile + interrupted, kiosk, in transit, etc.)
- **User skill level** — beginner / intermediate / expert / mixed

Every /craft command reads these and recalibrates. A high-consequence + mixed-skill + mobile-interrupted product gets fundamentally different output from a low-consequence + expert + desk-focused one.

## Working in a /craft project: directives for AI tools

Every AI tool doing work in this codebase should follow three conventions:

1. **Read foundation files before generating UI work.** PRODUCT.md, DESIGN.md, and FEATURES.md exist precisely so output is calibrated. Generating UI without reading them produces work that does not match the product.
2. **Update FEATURES.md when shipping a new feature.** Or invoke /craft features to reconcile. Stale FEATURES.md is the most common source of drift between code, marketing, and documentation; the convention exists to prevent it.
3. **Run a Judgment command before declaring work done on high-stakes flows.** PRODUCT.md names what counts as high-stakes (Operating Conditions: consequence). For high-consequence flows, /craft risk and /craft signal should run before merge.

## Recommended workflow

For pre-launch projects:

1. `/craft start` — establish PRODUCT.md and Operating Conditions
2. `/craft document` — capture DESIGN.md
3. `/craft features` — inventory current scope
4. `/craft scope` — cut what does not earn its place
5. `/craft compose`, `/craft signal`, `/craft risk`, `/craft responsive` — review what remains
6. `/craft coach` — pedagogical pass for the team
7. `/craft ship` — quality triage on the final scope

Skipping step 4 means later steps grade work that should have been cut. Skipping step 1 means every later step calibrates against generic defaults.

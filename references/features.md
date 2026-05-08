# features

Loaded when /craft routes to the `features` sub-command.

Capture what the product does, with enough hierarchy that the file reads like a product map rather than a flat list.

## When to use it

Run /craft features whenever the feature inventory might have drifted from reality. Most products lose track of their own feature list within months — marketing has one description, support has another, the README has a third, and the canonical answer lives in nobody's head. /craft features makes the inventory canonical and keeps it that way.

Reach for it:
- After shipping a new feature, to add it to the canonical inventory
- After deprecating something, to mark or remove it
- Periodically (monthly is reasonable) to reconcile drift
- At project start, to seed FEATURES.md from the current codebase
- Before writing marketing copy, sales decks, onboarding docs, or roadmaps that reference features
- When two team members describe the same feature differently and you need a tiebreaker

For strategic context (who, what, why), /craft start writes PRODUCT.md. For visual specification, /craft document writes DESIGN.md. /craft features is the third foundation file: what the product actually does.

## How it works

The pattern is "AI drafts, user confirms" — not "AI asks, user fills." On every invocation, do the maximum useful drafting from available context (codebase, PRODUCT.md, existing FEATURES.md), then surface the result for the user to review and tweak.

FEATURES.md must have hierarchy. Do not create one top-level section per detected feature. A flat list makes major workflows, supporting capabilities, and small implementation details look equally important, which makes the file hard to read and easy to misuse.

The four phases:

1. **Read PRODUCT.md.** Voice calibrates marketing copy. Skill level calibrates technical depth. User language calibrates feature naming.
2. **Scan the codebase.** Identify routes, primary components, user workflows, API surfaces, database-backed concepts, and repeated UI patterns. Distinguish user-facing capabilities from implementation details.
3. **Draft the hierarchy.** Group related features into feature areas, assign scale, draft descriptions, and mark uncertainty inline.
4. **Confirm.** Ask the user to review the hierarchy, scale choices, and any Needs Review questions. Do not treat a generated or migrated FEATURES.md as final until the user has had an obvious review moment.

## Required FEATURES.md shape

Every generated FEATURES.md should use this structure:

```markdown
# FEATURES.md

## Summary
[3-6 sentences. What the product does, who it serves, and the major capability areas.]

## Feature Map
| Area | Major features | Supporting features | Status |
|------|----------------|---------------------|--------|
| [Area name] | [2-5 major features] | [Important supporting features] | [shipped/planned/mixed] |

## Needs Review
- [Only include when needed. Use for uncertain hierarchy, naming, status, scale, or purpose.]

## [Feature Area]

> **Area summary:** [What this area enables and why it matters.]

### [Major Feature Name]

| Field | Value |
|-------|-------|
| Scale | major |
| Status | shipped \| planned \| deprecated |
| One-liner | [12 words maximum.] |
| Short | [2-3 sentences.] |
| Marketing | [Website-ready copy calibrated to PRODUCT.md.] |
| Technical | [Architecture, key dependencies, integration points, edge-case handling.] |
| Needs Review | [Only include this row when there is a concrete question for the user.] |

### [Supporting Feature Name]

| Field | Value |
|-------|-------|
| Scale | supporting |
| Status | shipped \| planned \| deprecated |
| One-liner | [12 words maximum.] |
| Short | [2-3 sentences.] |
| Marketing | [Website-ready copy calibrated to PRODUCT.md.] |
| Technical | [Architecture, key dependencies, integration points, edge-case handling.] |
```

Use top-level feature areas for product capabilities or workflows: Authentication, Onboarding, Core Workflow, Collaboration, Reporting, Administration, Billing, Integrations. Use feature entries inside those areas.

## Formatting and readability

FEATURES.md should be easy to scan as rich Markdown. Use visual hierarchy deliberately:

- Use `##` only for major document sections and feature areas.
- Use `###` only for individual feature names.
- Put each feature area's summary in a blockquote beginning with `> **Area summary:**`.
- Use a two-column table for every feature entry: `Field` on the left, `Value` on the right.
- Keep the left column labels stable and short: Scale, Status, One-liner, Short, Marketing, Technical, Needs Review.
- Do not use bold-label paragraphs like `**Short:** ...`; they blur together in long files.
- Leave one blank line between feature sections so the next `###` is visually distinct.
- Put long paragraphs only in the Value column. The table's left rail should make the mini sections obvious.

## Feature scale

Every documented feature gets a scale. Use scale to prevent tiny pieces from looking like pillars.

- **major** — A user-recognizable workflow, surface, or capability someone might name in onboarding, sales, support, or roadmap discussion. Examples: Guided Setup, Team Workspace, Scheduled Reports.
- **supporting** — A capability that matters, but primarily supports a major workflow. Examples: Role Invitations, Saved Filters, Submission Validation.
- **detail** — A small control, state, validation rule, helper, seed script, or implementation mechanism. Usually fold details into the parent feature's Technical field instead of giving them their own full entry.

When scale is ambiguous, mark it in the feature table:

```markdown
| Needs Review | Is this a supporting capability under Collaboration rather than a standalone major feature? |
```

## Description fields

Use these fields for major and supporting features:

- **Scale** — major or supporting. Details are usually folded into parent features.
- **Status** — shipped, planned, or deprecated. Three values only.
- **One-liner** — 12 words maximum. The elevator-pitch description. Used in command palettes, search results, sales sheets.
- **Short** — 2-3 sentences. Internal reference. What the feature does and why it matters in plain English.
- **Marketing** — website-ready copy. Voice and tone from PRODUCT.md. Used directly on landing pages, in changelogs, in announcement emails.
- **Technical** — engineer-facing. Architecture, key dependencies, integration points, edge-case handling. Used in onboarding new engineers and in code review context.

Why these fields: every feature ends up described in at least three places (marketing, internal docs, technical docs) and the descriptions drift apart. Capturing them in one canonical place means every other surface can pull from FEATURES.md instead of reinventing.

## Status: three values, mostly inferred

Status complexity is where most feature-tracking tools fail. People forget to update "in-progress," intermediate states proliferate, and the field becomes meaningless within a quarter. /craft features uses three values only:

- **shipped** — feature exists in code and is in production. Default for anything found in code.
- **planned** — feature is named in PRODUCT.md, the spec, or this file, but does not exist in code yet.
- **deprecated** — feature is being removed, has been removed, or is no longer supported.

"In-progress" is deliberately absent. The reconciliation diff catches in-progress work as either New (recently shipped) or Stale Status (planned-but-built). The user does not have to remember to mark or unmark anything.

## Review and uncertainty

Ask the user to review whenever:
- You are unsure whether something is a major feature, supporting feature, or detail
- A feature could belong under multiple areas
- A code surface exists but its product purpose is unclear
- A planned/deprecated status depends on product intent rather than code reality
- The generated hierarchy would shape how marketing, onboarding, or roadmap language gets written
- You have produced a first complete FEATURES.md from scratch
- You are migrating an existing flat FEATURES.md into the hierarchical format

Use concrete questions, not vague caveats. Put uncertain items in the **Needs Review** section at the top and in a `Needs Review` row on the relevant feature. If the uncertainty blocks a good hierarchy or accurate status, ask the user directly before finalizing.

Good:

```markdown
## Needs Review
- Should Saved Filters be a major feature, or a supporting feature under Reporting?
- Folder Organization appears in schema only. Is it still planned, or should it be removed/deprecated?
```

Bad:

```markdown
Some items may need review.
```

## Initial load, migration, and update pass

**Initial load (FEATURES.md does not exist).** Scan the codebase, draft Summary, Feature Map, feature areas, and entries. The user gets one clear review pass focused on structure first:

1. Are the feature areas right?
2. Are any major features actually supporting features?
3. Are any supporting details too small to document separately?
4. Are any Needs Review questions wrong or missing?

Only after the hierarchy is plausible should the user review wording.

**Migration pass (FEATURES.md exists but is flat).** Detect this when most features are top-level `##` sections and the file lacks `## Summary`, `## Feature Map`, and grouped feature areas. Do not append more flat entries. Reorganize the existing content into the required hierarchical shape:

1. Preserve every existing feature's One-liner, Short, Marketing, Technical, and Status text unless there is a clear reason to tighten wording.
2. Infer feature areas from user workflows, routes, nouns repeated across feature names, and PRODUCT.md language.
3. Assign Scale to each existing feature: major, supporting, or detail.
4. Fold detail-level entries into the nearest parent feature's Technical field instead of keeping them as standalone sections.
5. Add Summary, Feature Map, and Needs Review.
6. Ask the user to review the proposed structure before treating the migration as accepted.

For a flat inventory, the migration should produce generic product areas based on the repo:

```text
Authentication and Access
- Sign In and Session Management
- Role-Based Access

Core Workflow
- Workspace Creation
- Primary Task Flow
- Review and Submit

Collaboration
- Team Invitations
- Comments and Mentions
- Shared Activity

Reporting and Insights
- Dashboard Overview
- Exportable Reports
- AI-Assisted Summaries
```

That grouping is a draft, not a declaration. Mark doubts inline:

```markdown
| Needs Review | Should Saved Filters belong under Reporting instead of Core Workflow? |
```

**Update pass (FEATURES.md already exists and already has hierarchy).** Reconcile against the codebase and draft proposed changes for each drift item: New features, Stale Status, Orphans, and Hierarchy Drift. The user reviews each batch with a *does this look right?* confirmation, not field-by-field busywork.

## Diff mode: what it surfaces

Every invocation reconciles against the codebase. Surface four categories of drift:

1. **New** — features detected in code that do not appear in FEATURES.md. Draft full entries and propose area + scale.
2. **Stale status** — features whose status field no longer matches code reality. A feature marked `planned` but found in code should be `shipped`. A feature marked `shipped` but absent from code may have been deprecated.
3. **Orphan** — features in FEATURES.md with no corresponding code. Either the feature was never built (still `planned`), or it was removed without updating FEATURES.md (should be `deprecated`). Ask for resolution.
4. **Hierarchy drift** — features that are documented at the wrong level, duplicated across areas, or better folded into a parent feature.

This diff approach makes re-running the command always do useful work. Each invocation is a reconciliation pass, not a regeneration.

## Keeping it current: instruction to other AI tools

Other AI assistants doing work in this codebase should update FEATURES.md when shipping new features or deprecating existing ones. The reconciliation diff in /craft features will catch what other tools miss, but the cleanest workflow is: ship feature → update FEATURES.md as part of the work → run /craft features periodically to verify.

This skill file includes that directive explicitly so that AI tools reading it pick up the convention.

## How Operating Conditions reshape the output

The wedge. /craft features does not produce identical FEATURES.md regardless of context.

- **Voice from PRODUCT.md** → directly shapes marketing-level copy. A clinical, restrained voice produces different feature descriptions than a playful, consumer-facing voice. The command does not invent voice; it inherits it from PRODUCT.md.
- **Skill level "expert"** → technical descriptions can use domain jargon; one-liners can assume baseline familiarity.
- **Skill level "beginner" or "mixed"** → all descriptions, including technical, default to plainer language. One-liners avoid jargon entirely. Marketing copy explains rather than alludes.
- **Consequence "real-world harm" or "regulatory exposure"** → technical descriptions include compliance and audit-relevant details. A feature touching protected data names that explicitly. Marketing copy is more conservative about claims.
- **User language from PRODUCT.md** → feature names use the user's mental model, not internal labels. A "patient-record management" feature is named that, not "PatientRecordController.tsx" or "the patient module." The naming is the most important thing FEATURES.md gets right.

## Try it

    /craft features

Expected interaction (initial load):

```text
Reading PRODUCT.md...
Scanning codebase...

Drafted FEATURES.md with:
- 5 feature areas
- 11 major features
- 9 supporting features
- 4 items needing review

Please review the structure first:
1. Authentication and Access
2. Core Workflow
3. Collaboration
4. Reporting and Insights
5. Administration

Questions for you:
1. Should Saved Filters be supporting under Reporting, or promoted to a major feature?
2. Folder Organization appears in schema only. Is it still planned, or should it be removed/deprecated?

If this hierarchy looks right, I'll keep it and tighten the wording.
```

Expected interaction (update pass):

```text
Reading FEATURES.md... 5 areas, 19 features documented.
Scanning codebase... 21 features detected.

Drafting updates for 4 drift items.

NEW
[1] Bulk export
    Proposed area: Data Export
    Proposed scale: supporting
    Question: Should this belong under Records Management instead?

STALE STATUS
[2] Multi-clinician concurrent edit
    Currently: planned
    Code reality: shipped

HIERARCHY DRIFT
[3] Save-success confirmation
    Currently: standalone supporting feature
    Proposed: fold into Submission Validation and Editing as a detail.

ORPHAN
[4] Custom theme picker
    In FEATURES.md as: shipped
    Code reality: not found
    Question: Was this removed intentionally, or did it never ship?
```

## Pitfalls

- **Running it without PRODUCT.md.** Voice, skill level, and user language all come from PRODUCT.md. Without it, marketing copy reverts to generic SaaS prose that doesn't match the product. Start first.
- **Treating FEATURES.md as a wishlist.** Features in `planned` should be in PRODUCT.md or in an active roadmap. FEATURES.md is for things that are real or actively becoming real, not aspirational ideas with no commit attached.
- **Flattening the product.** Do not make every capability an H2. Start with Summary, Feature Map, and areas; then nest features under the right area.
- **Documenting details as features.** A validation rule, empty state, seed script, or signed-cookie helper usually belongs inside a parent feature's Technical field.
- **Using internal labels for feature names.** "Patient record management" is the user's mental model. "RecordsController" is implementation. The first is a feature; the second is code. /craft features uses the first, always.
- **Skipping the diff because "I just ran this last week."** The diff is cheap. The cost of stale FEATURES.md is high — every team that pulls from it gets wrong information. Re-run liberally.
- **Treating each description field as redundant.** They are not redundant. The one-liner goes in command palettes; the marketing description goes on the website; the technical description goes in onboarding. Same feature, different audiences. Each field earns its place.
- **Confirming drafts without reading them.** The AI is competent at drafting from code + PRODUCT.md but it is not infallible. Structure, scale, and naming shape every downstream use of FEATURES.md. Questions in **Needs Review** and feature-level `Needs Review` rows are explicit about uncertainty; everything else still earns a read.

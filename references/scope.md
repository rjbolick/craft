# scope

Loaded when /craft routes to the `scope` sub-command.

Does every feature, every screen, every flow step earn its place?

## When to use it

AI overbuilds. Given a request, the model tends to add "completeness" features, edge-case handling, settings nobody asked for, configurations for theoretical users, and depth not warranted by the actual user problem. /craft scope pushes back on that tendency by making each piece of work justify its presence.

Reach for it:
- After AI produces a feature, screen, or flow that feels bigger than asked for
- When reviewing a product spec before building, to cut early
- When a user flow has grown to 5+ steps and feels heavy
- When a settings page has 20+ options
- Before committing to a feature, to test if it earns its place
- When PRODUCT.md says "minimal" or "focused" and the build doesn't reflect that

For quality calibration on already-decided scope, /craft ship. /craft scope decides what should exist; /craft ship decides what has to be solid.

## How it works

The command applies three lenses in order. Each generates findings. Findings sort into three buckets.

### Lens 1: User-problem fit

For each feature, screen, or element, ask:
- What problem named in PRODUCT.md does this solve?
- Is the problem real (named, evidenced) or hypothetical ("users might want")?
- Is the problem already solved adjacent? (A "Help" button next to a tooltip system; a search bar in a five-item list)
- Would removing this make the user's life worse, or just remove a feature?

Features answering hypothetical problems → Cut. Features duplicating adjacent solutions → Cut. Features clearly serving named problems → carry forward to Lens 2.

### Lens 2: Depth calibration

For each feature that passed Lens 1, ask:
- Is this at the right depth, or over-engineered?
- Are there options, settings, or configurations nobody requested?
- Could 80% of the value come from 20% of the depth?
- What is the AI-generated completeness here?

Common AI overbuild patterns this lens catches:
- Settings pages with 30+ options when 5 cover 95% of use
- Empty states with custom illustrations when defaults communicate state fine
- Form validation with 12 rules when 3 catch all real errors
- Onboarding tours covering features users won't hit for weeks
- Admin tools with full CRUD when read-only would do
- Dashboards with 8 charts when 2 answer the actual question
- Confirmation dialogs on actions that aren't actually risky
- Theme/personalization features for users who didn't ask

Over-engineered features → Simplify with a specific recommendation. Right-depth features → carry forward to Lens 3.

### Lens 3: Flow simplicity

For each user flow, ask:
- How many steps? Is each step earning its place?
- Are there confirmation dialogs that don't reduce real risk?
- Are there transition screens that just sit between actions?
- Could two steps be merged?
- Could a step be removed entirely without losing safety?

Flows with unnecessary steps → Simplify with specific cuts. Direct flows → Keep.

### The three buckets

- **Cut** — doesn't earn its place. No named user problem, or the problem is already solved adjacent.
- **Simplify** — earns place but is over-built. Specific recommendation for the cut.
- **Keep** — earns place at right depth. Often the smallest bucket; that's the point.

## How Operating Conditions reshape the bar

The wedge. /craft scope does not apply the same threshold regardless of context.

- **Consequence "real-world harm" or "regulatory exposure"** → Bar rises. Anything not directly serving the high-stakes flow gets cut harder. Compliance features stay; nice-to-haves go. The product gets narrower and sharper.
- **Skill level "beginner" or "mixed"** → Flow simplicity weighted heaviest. A beginner cannot navigate a 7-step flow. Cut steps aggressively; merge where possible.
- **Skill level "expert" + "desk + focused"** → Depth more permitted. Power-user features and density earn place when matched to a named expert workflow. Still cut hypothetical features.
- **Stage pre-launch or small user base** → Ruthless cuts. Anything not on the path to first 100 real users is cut. Settings, theming, admin tools, advanced features defer entirely. The bar is "do we need this for the first cohort?"
- **Stage established or scaling** → Bar relaxes slightly. Mature products earn the right to depth — but only against named user requests, not AI-imagined ones.

A high-consequence + mixed-skill + pre-launch project gets a long Cut list and a short Keep list. A low-consequence + expert + established product gets a more balanced output. Both are correct under their conditions.

## Where it sits in the workflow

/craft scope runs early — ideally before /craft risk, /craft signal, and /craft compose. If the spec is overbuilt, those commands surface findings on items that should not exist, and the team wastes effort fixing things scope would have cut.

The recommended pre-launch sequence:

1. /craft start (foundation)
2. /craft document (foundation)
3. **/craft scope (cut what shouldn't exist)**
4. /craft risk, /craft signal, /craft compose (review what remains)
5. /craft coach (pedagogical pass)
6. /craft ship (quality triage on the surviving scope)

Skipping step 3 means later steps grade work that should not have been built.

## Try it

    /craft scope the patient-record management feature

Expected output shape:

    Operating Conditions: real-world harm + mixed skill + interrupted environment
    Stage: pre-launch, ten-week runway

    CUT (doesn't earn its place)

      [1] Theme picker with 8 color schemes
          User problem: none named. Clinicians use hospital-issued
          workstations; theme is not a problem they have.
          AI overbuild: completeness pattern. Generated because settings
          pages "usually have themes," not because users asked.
          Hours saved: ~12

      [2] "Recently viewed records" sidebar
          User problem: hypothetical. PRODUCT.md says workflow is
          single-record-focused; recently-viewed answers a problem nobody
          named.
          Adjacent solution: browser back-button already works.
          Hours saved: ~8

      [3] Bulk-edit mode for multiple records
          User problem: not in PRODUCT.md, and incompatible with the
          high-consequence operating context. Bulk operations on patient
          records are how disasters happen.
          Hours saved: ~20

    SIMPLIFY (earns place but over-engineered)

      [4] Patient settings page (currently 23 options)
          User problem: yes — clinicians need notification preferences,
          default view, language, timezone.
          Overbuild: 19 of 23 options serve no named problem.
          Recommendation: cut to 4 visible options. Drop the rest entirely
          rather than burying them under "Advanced" — adding back is
          cheap if a real request comes in.

      [5] New-record creation flow (currently 6 steps)
          User problem: yes — clinicians create records.
          Overbuild: steps 3 and 4 are confirmation dialogs that don't
          reduce risk (no irreversible action). Step 6 is "review" but
          mixed-skill users won't catch errors at step 6 they missed
          at step 5.
          Recommendation: collapse to 3 steps. Patient identification →
          record content → save with explicit success state.

      [6] Form validation (14 rules across 6 fields)
          User problem: yes — input correctness matters in this domain.
          Overbuild: 5 rules catch all real errors observed in similar
          workflows. Other 9 catch theoretical edge cases.
          Recommendation: ship the 5; add the others when real users
          hit the gaps.

    KEEP (earns place at right depth)

      [7] Patient-name disambiguation in selector
          (Critical, named in PRODUCT.md, solved at right depth.)

      [8] Save with explicit success confirmation

      [9] Audit log of edits (regulatory requirement)

      [10] Core validation rules covering required fields and format

    SCOPE SUMMARY

      Cut: 3 features (~40 hours saved)
      Simplify: 3 features (~25 hours saved)
      Keep: 4 features (no change)

      Net runway impact: ~65 hours returned. Recommend redirecting to
      LOAD-BEARING items surfaced by /craft ship.

## Pitfalls

- **Running it without PRODUCT.md.** The whole bar depends on Operating Conditions and named user problems. Without them, scope can't distinguish "real user problem" from "hypothetical user might want." Start first.
- **Treating Cut as a personal judgment.** It is a stakes-and-evidence call: no named problem, or the problem is already solved. The team member who built the feature is not on trial.
- **Treating Simplify as a defer.** Simplify means the cut happens now; the deeper version goes on a wishlist that activates when a real user asks. Half-Simplify with the deeper version still in the codebase costs as much as Keep.
- **Cutting things users have actually asked for.** Scope cuts AI overbuild and spec creep. Real user requests are different — they get scrutinized by Lens 2 (right depth) but not by Lens 1 (user-problem fit). Note evidence of user requests in PRODUCT.md or before running.
- **Running scope after build is mostly done.** The command works pre-build or early-build. Running it on completed work means cutting code that already exists, which is more expensive than not building it. Run early; run often.
- **Confusing scope with /craft ship.** Scope decides what should exist. Ship decides what has to be solid. Both are restraint commands; they answer different questions in sequence.

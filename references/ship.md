# ship

Loaded when /craft routes to the `ship` sub-command.

What's load-bearing here, and what can be janky? The 0→1 quality bar.

## When to use it

Run /craft ship when the calendar is finite. You have six weeks, one engineer, and a launch date. Most quality tools tell you to make everything bulletproof. Ship inverts that: it triages what actually has to be solid for the stakes you're operating at, and explicitly permits jank where jank is fine.

Reach for it:
- Pre-launch when the todo list is longer than the runway
- After /craft risk surfaces 30 findings and you need to know which 5 are blockers
- When polish hours are stacking up on flows nobody hits
- When a stakeholder asks "is this ready to ship?" and you need a defensible answer
- After /craft coach identifies improvements but the calendar can't take all of them

Ship is for triage on what's actually load-bearing. It does not produce a production-hardening pass — that is a different kind of work.

## How it works

Ship runs a triage pass and sorts every observation into one of three buckets, each with rationale tied to Operating Conditions:

1. **Load-bearing** — must be solid before ship. The core happy path. Auth and data integrity. Anything with regulatory exposure. The first 30 seconds of new-user experience. Anything visible on every page. These cannot be janky; if they are, the launch is broken.

2. **Acceptable janky** — defer. Edge cases hitting <5% of users. Polish on infrequent flows. Onboarding refinements past the minimum-viable arc. Internationalization if you're not international yet. Keyboard shortcuts beyond standard expectations. These are real improvements; they're just not the ones blocking ship.

3. **Stop polishing** — wasteful effort right now. Pixel-perfect responsive on devices below 1% usage. Admin tools before there are customers. Settings UI for features no one's asked for. Animation polish on flows that should be near-instant anyway. Empty states for paths users won't reach pre-launch. Naming this category matters because the time spent here is time NOT spent on load-bearing items.

The flow runs in four phases:

1. **Read PRODUCT.md.** Operating Conditions decide what counts as load-bearing. A high-consequence regulated product has more load-bearing items than a consumer app. A beginner-skill product makes onboarding load-bearing; an expert product makes it deferrable.
2. **Aggregate prior /craft findings.** If /craft risk, /craft signal, /craft compose, or /craft coach have run, ship reads their findings and triages them. If no prior findings exist, ship runs its own scan covering the same ground at lower depth.
3. **Triage.** Every item gets sorted into load-bearing, acceptable janky, or stop polishing. Each call is justified explicitly: "this is load-bearing because Operating Conditions specify regulatory exposure," not "this is important."
4. **Surface tradeoffs.** When the load-bearing bucket is too full to fit the calendar, ship names the structural choice. "If you can't fix all six load-bearing items in three weeks, narrow the launch scope; don't lower the bar on individual items."

## How Operating Conditions reshape the review

The wedge. /craft ship does not make the same triage calls regardless of context — Operating Conditions decide what counts as load-bearing.

- **Consequence "real-world harm" or "regulatory exposure"** → Load-bearing expands. Compliance items, consent flows, audit logging, error recovery for destructive actions are all load-bearing. "Acceptable janky" applies only to peripheral aesthetic concerns; nothing safety-relevant lands there.
- **Consequence "reversible inconvenience"** → Load-bearing contracts. The core happy path is load-bearing; most error states can be acceptable janky; polish on edge cases is often "stop polishing."
- **Skill level "beginner" or "mixed"** → Onboarding, affordance, error recovery, and recognition over recall become load-bearing. A beginner who hits a polished error message but a missing onboarding flow churns; one who hits a janky error message but reaches the aha moment activates.
- **Skill level "expert" + "desk + focused"** → Onboarding becomes acceptable janky; keyboard shortcuts and density become load-bearing. Expert users tolerate rough edges but punish friction in their daily flow.
- **Stage pre-launch or small user base** → Admin tools, internal dashboards, and features for hypothetical power users move toward "stop polishing." Build what gets you to your first 100 real users; everything else waits.

A high-consequence + mixed-skill + pre-launch project gets a load-bearing bucket of 8-12 items and a small "stop polishing" list. A low-consequence + expert + post-launch project gets a load-bearing bucket of 3-5 items and a large "stop polishing" list. Both are correct calls under their conditions.

## Try it

    /craft ship the v1 launch of the patient-record form

Expected output shape:

    Operating Conditions: real-world harm + mixed skill + interrupted environment
    Stage: pre-launch, three-week runway

    LOAD-BEARING (must be solid before ship)

      [1] Patient-name disambiguation in the selector
          Why: real-world harm. Selecting the wrong patient is the
          highest-severity slip in the product. Cannot be janky.
          Status: not done. /craft risk flagged this as criticality 18.

      [2] Save-success confirmation
          Why: clinicians under interruption need explicit confirmation
          that the save committed. Silent success reads as "did I click
          that?" Cannot be janky in this environment.
          Status: needs work. Currently a 1.5s toast that disappears.

      [3] Audit log for record edits
          Why: regulatory exposure. Required for HIPAA-class workflows.
          Cannot be deferred to v2; auditability is a v1 condition.
          Status: missing.

      [...]

    ACCEPTABLE JANKY (defer; address later)

      [12] Animation polish on tab transitions
          Why: aesthetic preference, not load-bearing. Mid-task clinicians
          will not notice and would not be helped if they did.
          Defer to: post-launch polish sprint.

      [13] Custom illustrations for empty states
          Why: charming but not gating. Default illustrations communicate
          state adequately for the audience.
          Defer to: when a designer has bandwidth.

      [...]

    STOP POLISHING (effort that won't move outcomes)

      [22] Theme customization for individual users
          Why: nobody's asked for it. The user is a clinician at a specific
          facility; theming is not a need. Hours spent here should redirect
          to LOAD-BEARING [1] and [2].

      [23] Keyboard shortcut tutorial overlay
          Why: skill level is "mixed," and beginners will not use shortcuts.
          Build the shortcuts; skip the tutorial. Standard documentation
          is enough for the experts who want them.

      [...]

    TRADEOFF

      Load-bearing has 9 items; the three-week runway fits 6-7 cleanly.
      Recommend: narrow v1 to single-clinician workflows; defer multi-
      clinician concurrent edit (LOAD-BEARING [4]) to v1.1 by restricting
      to a single editor per record at the data layer. Cuts scope by one
      item and removes the concurrency-handling work that was the largest
      unknown.

## Pitfalls

- **Running it without PRODUCT.md.** Ship triage is entirely Operating-Conditions-dependent. Without them, the review falls back to generic SaaS triage and produces calls that are wrong for high-consequence projects. Start first.
- **Treating "acceptable janky" as a permanent bucket.** It is a deferral list, not a wontfix list. Items here belong in the post-launch backlog with clear conditions for when to address them.
- **Ignoring "stop polishing."** This is the bucket the team resists most because someone is invested in the items. Naming the cost — "this hour is not on a load-bearing item" — is the whole point.
- **Asking ship to lower the load-bearing bar.** Ship calibrates to Operating Conditions, not to the calendar. If the load-bearing list does not fit, narrow the scope of v1; do not lower the bar on individual items. Shipping a regulated product with deferred audit logging is not a triage call, it is a violation.
- **Running ship before /craft risk and /craft signal.** Ship triages findings; if there are no findings yet, run risk and signal first so ship has something to triage. Ship does cover similar ground in its own scan, but at lower depth.
- **Confusing ship with a feature-cut tool.** Ship triages quality, not features. "Should we build feature X?" is a product question. "Given we're building X, what has to be solid?" is the ship question.

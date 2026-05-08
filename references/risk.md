# risk

Loaded when /craft routes to the `risk` sub-command.

What fails when this UI is wrong, and what is it going to cost?

## When to use it

Run /craft risk on any surface where a user mistake has real cost: forms that commit irreversible actions, flows that move money, anything regulated, anything where the user is interrupted or under pressure. The command surfaces failure modes, missed edge cases, and consequence-blind defaults that aesthetic and accessibility reviews skim over.

Reach for it:
- Before shipping any high-consequence flow (delete, send, pay, submit, publish)
- After /craft start sets consequence to "lost work," "real-world harm," or "regulatory exposure"
- When a bug report starts with "the user thought they were doing X but actually..."
- When users keep making the same mistake — the design is the bug

For aesthetic review, reach for /craft coach. For applied UI fundamentals, /craft compose. /craft risk is failure-mode thinking specifically.

## How it works

The command runs an FMEA-style review (failure mode and effects analysis, adapted from medical-device design) on the target. Each finding gets three scores and a recommendation:

- **Severity (1-3)** — how bad is the consequence when this UI is wrong?
  - 1 = annoying, recoverable in seconds
  - 2 = lost work, requires re-doing or support contact
  - 3 = real-world harm, regulatory exposure, irreversible damage
- **Probability (1-3)** — how likely is the failure?
  - 1 = rare, requires unusual user behavior
  - 2 = occasional, possible under normal use
  - 3 = likely, anyone could trip it
- **Detectability (1-3)** — how likely is it that the user notices BEFORE harm?
  - 1 = obvious, user catches it immediately
  - 2 = sometimes caught, sometimes missed
  - 3 = often invisible until consequences arrive

Criticality is S × P × D. Higher is worse. A 3×3×3 (max 27) is a release-blocker; a 1×1×1 is barely worth flagging. The exact bands /craft uses:

- **18+ critical** — fix before ship
- **8-17 significant** — fix this sprint
- **1-7 minor** — FYI, address when there's time

The flow runs in five phases:

1. **Read PRODUCT.md.** Operating Conditions calibrate every score. The same form scores differently in a low-stakes consumer context than in a high-stakes regulated one. Without Operating Conditions, the review falls back to generic averages and produces a generic report.
2. **Scan.** Identify every interactive element, state transition, destructive action, multi-step flow, form input, and irreversible operation in the target.
3. **Failure-mode identification.** For each interactive element, walk five categories:
   - **Input errors:** wrong format, out of range, missing required, ambiguous defaults
   - **State errors:** stale data, race conditions, optimistic UI that lies, double-submission
   - **User errors:** wrong button, wrong field, slip vs. mistake, dark-pattern adjacency
   - **System errors:** network failure, permission denied, server error, rate limit
   - **Misuse:** the user using the tool in a way it was not designed for, with bad outcomes
4. **Edge-case discovery.** What cases is this NOT handling? Long text, empty data, special characters, large numbers, RTL, slow networks, accessibility tech, concurrent users, partial states.
5. **Recovery review.** For each failure mode, can the user undo? Notice the mistake? Get back to a safe state? A failure with a one-click recovery is half as critical as one without.

## How Operating Conditions reshape the review

The wedge. /craft risk does not produce the same review for the same code regardless of context — Operating Conditions recalibrate every score:

- **Consequence "real-world harm" or "regulatory exposure"** → Severity anchors higher. A 14-character truncation that hides the last name of a patient is severity 3, not severity 1. Recovery paths are mandatory, not nice-to-have. Misuse cases get explicit attention.
- **Skill level "beginner" or "mixed"** → Probability rises. Slips that an expert avoids will trip a beginner. Affordance ambiguity becomes a failure mode, not a polish issue.
- **Environment "interrupted" or "in transit"** → Detectability rises (failures get harder to catch). Confirmation flows that work fine for a focused user fail when the user is glanced-at-and-pulled-away.
- **Regulatory exposure** → The question shifts from "what's reasonable?" to "what's defensible?" Misuse and off-label-use cases that an unregulated review would skip become critical findings.

An expert + focused + low-consequence project gets a short, advisory review with mostly criticality < 8. A mixed-skill + interrupted + real-world-harm project gets a sharper review with multiple criticality 18+ findings to clear before ship.

## Try it

    /craft risk the patient-record form

Expected output shape:

    Operating Conditions: real-world harm + mixed skill + interrupted environment

    CRITICAL (criticality 18+, fix before ship)

      [1] "Delete record" button adjacent to "Save record"
          S3 × P3 × D2 = 18
          Risk: User intends save, hits adjacent destructive action.
          Recovery: None. Deletion is permanent.
          Fix: Separate destructive actions spatially and visually.
               Require typed confirmation of patient's last name.
               Log the action; offer 60s reversal window.

      [2] Patient name truncates without indication at 22 chars
          S3 × P2 × D3 = 18
          Risk: Last names cut off; clinician selects wrong patient.
          Recovery: None — selection is irreversible without further notice.
          Fix: Show full name on hover. Expand to 40 chars in selector.
               Require explicit disambiguation when last names overlap.

    SIGNIFICANT (criticality 8-17, fix this sprint)
      [3] ...

    MINOR (criticality 1-7, address when there's time)
      [...]

    EDGE CASES NOT HANDLED
      - 5,000-char free-text note overflows without truncation strategy
      - Patient names with special characters (apostrophes, accents)
      - Concurrent edits by two clinicians on same record

Hand critical findings to /craft signal (affordance fixes), /craft compose (hierarchy and grouping fixes), or apply directly. /craft risk diagnoses; it does not auto-implement.

## Pitfalls

- **Running it without PRODUCT.md.** The review falls back to generic severity. Start first.
- **Treating low-criticality findings as blockers.** They are FYI items, not release-blockers. Criticality 18+ first; 8-17 next; below 8 only with time.
- **Confusing /craft risk with /craft signal.** Risk catches the slip after it happens. Signal prevents the slip from being possible. Both belong in a complete review.
- **Skipping the recovery review.** A failure with one-click recovery is half as critical as one without. Always score recovery, always recommend a path.
- **Scoring detectability as if the user is paying attention.** Operating Conditions decide attention. "Mobile + interrupted" raises detectability scores; "desk + focused" lowers them.
- **Assuming "add a confirmation dialog" fixes everything.** Confirmation fatigue is real. Risk recommends specific patterns: typed confirmation for severity 3, reversal windows for severity 2, no confirmation for severity 1 + recoverable.
- **Auto-implementing fixes.** /craft risk is diagnostic. The user (or another command) implements. Mixing diagnosis and implementation in one pass loses the audit trail that high-consequence projects often need.

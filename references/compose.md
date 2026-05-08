# compose

Loaded when /craft routes to the `compose` sub-command.

Apply the fundamentals of visual composition to a UI.

## When to use it

Run /craft compose when a UI feels off and the issue isn't typography or aesthetics. Compose looks at structural problems: things that aren't grouped when they should be, hierarchies that don't lead the eye, layouts with no rhythm, information architecture that obscures rather than reveals.

Reach for it:
- When a layout is "technically fine" but users still don't know where to look
- When the same content reads as cluttered in one section and sparse in another
- When users miss features that are present because the grouping doesn't telegraph relationship
- When a redesign is being scoped and you want a structural baseline before any visual moves

For aesthetic intentionality, /craft coach. /craft compose is structural — the design-school fundamentals applied to whatever's on the screen.

## How it works

The command walks six canonical principles in order. They build on each other; later principles assume earlier ones are working.

1. **Gestalt grouping.** The five rules of how the eye groups elements: proximity (close = related), similarity (same treatment = same kind), continuation (aligned = connected), closure (incomplete forms still read as whole), figure/ground (foreground/background distinction). Violations show up as elements that *look* related but aren't, or *aren't* related but look it.

2. **Visual hierarchy.** What does the eye land on first, second, third? Achieved through size, weight, contrast, color, position, and isolation — typically two or three of these working together. The squint test: blur your eyes, can you still tell what matters?

3. **Balance.** Symmetric or asymmetric, the visual weight has to distribute. Heavy elements at the bottom-right with nothing balancing them creates a tilt the user feels but can't name. Asymmetric balance is fine — and often more sophisticated — but requires intentional counterweights.

4. **Rhythm.** Repetition with variation. Cards that all look identical create monotony; cards that vary randomly create chaos. Good rhythm alternates: tight grouping then generous breathing, regular intervals then a deliberate break for emphasis.

5. **Information architecture.** Are related items grouped under the right labels? Does the navigation reflect the user's mental model or the org chart? Are there orphan items that don't belong anywhere? IA failures cascade into every other layer; if the structure is wrong, no amount of typographic polish fixes it.

6. **Compositional structure.** The overall arrangement: focal points, breathing room, edge treatment, the relationship between content and frame. Whether the page feels composed (like a magazine spread) or assembled (like a stack of components).

The flow runs in three phases:

1. **Read PRODUCT.md.** Operating Conditions decide density, prominence, and grouping requirements. Beginner + interrupted environment demands stronger Gestalt grouping and more obvious hierarchy. Expert + focused permits subtler treatments.
2. **Inventory and assess.** For the target surface, walk all six principles. Note violations with specific examples.
3. **Recommend fixes.** Each finding gets a specific fix — not "improve hierarchy" but "drop the H1 from 32px to 28px, raise the eyebrow text from 11px to 13px, and remove the secondary CTA's drop shadow."

## How Operating Conditions reshape the review

The wedge. /craft compose does not require the same density or prominence regardless of context — Operating Conditions reshape what compositional success looks like.

- **Skill level "beginner" or "mixed"** → Gestalt grouping must be stronger. Items should not be ambiguously grouped; the design has to remove guesswork. Hierarchy needs more contrast — at least two dimensions of differentiation between adjacent levels (size + weight, not size alone).
- **Environment "interrupted" or "mobile"** → Squint test must pass at a glance. Focal points have to be obvious without scanning. Density tightens but never to the point of failing the squint test.
- **Consequence "real-world harm" or "regulatory exposure"** → Critical actions (delete, submit, irreversible) get hierarchical isolation, not just typographic emphasis. Spatial separation is part of compositional safety. Adjacent destructive actions are a compose-level failure, not just a polish issue.
- **Skill level "expert" + "desk + focused"** → Subtler hierarchy permitted. Information density can go higher. Asymmetric balance and editorial-style composition are permissible where they would fail for a mixed audience.

A high-consequence + mixed-skill review names compositional failures that an expert + focused review would tolerate. The principles are universal; the thresholds are not.

## Try it

    /craft compose the dashboard

Expected output shape:

    Operating Conditions: lost work + intermediate skill + desk + focused

    GESTALT GROUPING

      [1] Recent activity and Notifications visually merge
          Violation: Proximity. The 8px gap between sections is the
          same as the gap between items within each section.
          User assumption: One unified list of "things that happened."
          Fix: Increase between-section gap to 32px. Add a hairline
          divider only if the spacing alone reads as ambiguous.

      [2] Action buttons in toolbar styled identically to filter chips
          Violation: Similarity. Same shape, same color, same size.
          User assumption: Both groups do the same kind of thing.
          Fix: Filter chips drop to ghost treatment (no fill).
          Action buttons retain filled treatment. The visual class
          becomes diagnostic of function.

    VISUAL HIERARCHY

      [3] Page title, section title, and card title all 18px / 600
          Violation: No hierarchical contrast between three levels.
          Squint test: fails — everything reads as the same level.
          Fix: Page title 28px / 700, section 20px / 600, card 16px / 600.
          Two dimensions of differentiation per level (size + weight or
          size + color), never one alone.

    BALANCE [...]
    RHYTHM [...]
    INFORMATION ARCHITECTURE [...]
    COMPOSITIONAL STRUCTURE [...]

Hand findings to the implementing layer (or apply directly). /craft compose diagnoses; the user implements.

## Pitfalls

- **Running it without PRODUCT.md.** Density and prominence requirements are Operating-Conditions-dependent. A subtle hierarchy that works for an expert audience fails for a beginner one. Start first.
- **Treating compose as the typography command.** /craft compose addresses hierarchy and grouping; the actual type pairing and scale belong to a dedicated typography pass or hand-tuning. Compose says "the H1 needs more weight than the H2"; it does not pick the typeface.
- **Confusing /craft compose with /craft coach.** Compose applies specific principles structurally. Coach does pedagogical critique across the whole design including aesthetic and brand judgment. The same finding might appear in both, framed differently.
- **Running it on broken IA.** If the navigation reflects the org chart instead of the user's mental model, no amount of Gestalt grouping rescues it. IA failures cascade — fix structure first, then visual.
- **Squint-test cheating.** Some designers run the squint test mentally and rationalize what they see. Actually blur your eyes (or the screen) and look. If hierarchy is real, it survives the test.

---

About: /craft compose codifies the visual composition framework Ryan Bolick applies across the courses he teaches at Duke, the companies he builds, the tools he ships, and the teams he advises.

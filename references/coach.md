# coach

Loaded when /craft routes to the `coach` sub-command.

Office hours, not code review. Critique that teaches as it evaluates.

## When to use it

Run /craft coach when the goal is understanding, not just fixing. The command walks a UI through canonical design principles (Nielsen, Gestalt, Norman, Tufte, Krug) and explains the underlying principle behind every observation, so the developer or designer learns the framework instead of just collecting fixes.

Reach for it:
- When teaching design judgment to engineers, junior designers, or founders building their own UI
- When a critique should produce understanding the designer carries forward, not just a checklist
- During design review where the rationale matters as much as the verdict
- When a stakeholder asks "what's wrong with this?" and you want the answer to make them better, not defensive
- In workshop or classroom settings where the artifact IS the lesson

For failure-mode review, /craft risk. For applied composition review, /craft compose. /craft coach is broader and more pedagogical than either — the command for when the goal is teaching, not just fixing.

## How it works

The command walks the target UI through canonical design principles and produces a critique structured for learning. Every finding has the same shape:

1. **Observation** — what's happening on screen, named specifically. ("The error message reads 'Something went wrong.'")
2. **Principle** — the named, canonical principle being violated. ("Nielsen's 'Help users recognize, diagnose, and recover from errors' — error messages should be plain language, indicate the problem precisely, and constructively suggest a solution.")
3. **Recommendation** — the specific fix, written so the designer could apply the principle to other situations later. ("Replace with: 'Couldn't save your changes — your session expired. Sign in again to continue.' Specific cause + plain language + recovery path. Apply this pattern to all error states in the flow.")

The output is structured to teach. The principle gets named every time, even when it repeats — repetition is the point. A designer who runs /craft coach on three projects starts to recognize the principles before the AI surfaces them.

The flow runs in three phases:

1. **Read PRODUCT.md.** Operating Conditions decide which principles get emphasis. A high-consequence project weights "error prevention" and "recognition over recall" heavily; a marketing surface weights "aesthetic and minimalist design" and "match between system and real world."
2. **Walk the principles.** Six canonical sources, applied in order:
   - **Nielsen's 10 usability heuristics** — the foundation. Every UI gets walked through these.
   - **Gestalt principles** — proximity, similarity, continuation, closure, figure/ground (overlaps with /craft compose; coach addresses them as critique, compose addresses them as fix).
   - **Norman's affordance/signifier/mapping/feedback** — Don Norman's *Design of Everyday Things* (overlaps with /craft signal; same overlap pattern).
   - **Tufte on information density** — chart junk, data-ink ratio, small multiples. Critical for any data-displaying surface.
   - **Krug's "Don't Make Me Think"** — three laws of usability, with attention to the convention-versus-clarity tradeoff.
   - **Cognitive load** — the count of decisions, options, and items the user is asked to process at once.
3. **Surface findings + close with questions.** Findings ordered by importance (not by principle). The closing section is provocative questions the interface itself cannot answer — the things the designer has to decide.

## How Operating Conditions reshape the review

The wedge. /craft coach does not weight the same principles equally regardless of context — Operating Conditions reshape emphasis.

- **Consequence "real-world harm" or "regulatory exposure"** → Error prevention, recognition over recall, and visibility of system status weighted heaviest. Aesthetic principles still surface but lower in the output.
- **Skill level "beginner" or "mixed"** → Match between system and real world (familiar metaphors), help and documentation, and consistency weighted heaviest. Convention is non-negotiable; clever-but-novel is a finding.
- **Skill level "expert"** → User control and freedom, flexibility and efficiency of use weighted heaviest. Conventions can be broken when the break serves expert workflows.
- **Environment "interrupted" or "mobile"** → Visibility of system status, error prevention, and minimalist design weighted heaviest. Anything requiring sustained focus to understand is a finding.

The principles are universal; what counts as a violation depends on context. /craft coach makes that calibration explicit in the output.

## Try it

    /craft coach the patient-record form

Expected output shape:

    Operating Conditions: real-world harm + mixed skill + desk + focused
    Emphasis: error prevention, recognition over recall, system status

    WHAT'S WORKING

    The form structure is logical. Required fields are marked. The save action
    is positioned where users expect it (top-right). Form-level validation
    runs on submit, not field-by-field — appropriate given the audience.

    PRINCIPLE-LEVEL FINDINGS

      [1] Patient-name field accepts free text with no disambiguation
          Principle (Nielsen): Error prevention — the best error message is
          the one that never appears. Eliminate error-prone conditions, or
          check for them and present a confirmation.
          Observation: Two patients with similar names will produce a slip
          that the form has no way to catch. The user types and submits;
          the system trusts the input.
          Recommendation: Convert to a typeahead with patient ID and DOB
          shown alongside name. Require explicit selection from a result.
          Block manual entry of names not matched to a record. The pattern
          generalizes: any time a free-text field references a real-world
          entity, replace it with a constrained selection from that entity's
          authoritative source.

      [2] Submit button shows no system status during async save
          Principle (Nielsen): Visibility of system status — the system
          should always keep users informed about what is going on,
          through appropriate feedback within reasonable time.
          Observation: Click → silence → either a success toast or a
          generic error. Mid-flight, the user has no signal that anything
          is happening, leading to double-submission attempts.
          Recommendation: Disable the submit button on click; show a
          spinner inside the button (not next to it); change the label
          to "Saving..." Feedback during the action, not just after.
          Apply this pattern to every async action in the product —
          state during, not just outcome.

      [3] Field labels positioned to the left of inputs, color #999 on white
          Principle (WCAG / Nielsen "Aesthetic and minimalist design"):
          Aesthetic minimalism does not override readability. Functional
          text must meet contrast minimums.
          Observation: 1.4:1 contrast ratio. Below WCAG AA threshold of
          4.5:1 for normal text. Functional, not decorative.
          Recommendation: Darken to #666 minimum, #444 preferred. Apply
          across all functional labels. Aesthetic text (decorative quotes,
          watermarks) can stay light; labels users must read cannot.

    [...]

    QUESTIONS THE INTERFACE CAN'T ANSWER

      - When two clinicians edit the same record concurrently, what's the
        intended resolution model? The current UI doesn't reveal a choice
        was made.
      - Is the "Delete record" action ever the right choice for a clinician,
        or should it be an admin-only operation? The form gives every user
        the same destructive power.
      - The "Save" button commits to the patient's permanent record. The
        word "Save" is too gentle for that consequence. What's the correct
        verb?

## Pitfalls

- **Running it without PRODUCT.md.** Principle weighting falls back to generic emphasis. The same finding gets the same priority regardless of stakes, which produces unhelpful reviews for high-consequence projects. Start first.
- **Skimming the principle line.** The principle name is the lesson. Reading only the observation and recommendation collects fixes without learning the framework. The principle is the durable artifact; the recommendation is the one-time fix.
- **Treating coach as a scoring tool.** /craft coach does not score against Nielsen heuristics 0-4. Scoring invites gaming. Coach gives qualitative judgment because design judgment is qualitative.
- **Ignoring "What's working."** The opening section is not throat-clearing. It identifies what to preserve through any redesign. A common failure mode is fixing findings while breaking what was already working.
- **Skipping the closing questions.** They surface the things the interface alone cannot decide — the genuine design choices that need a human. Often the most important part of the review.

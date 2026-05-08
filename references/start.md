# start

Loaded when /craft routes to the `start` sub-command. `/craft brief` is accepted as an alias, but `start` is the public command name.

Start /craft in a project. Teach it who the product is for, what's at stake, and what to do next.

## When to use it

Run /craft start once at the start of a project. Without it, every other /craft command produces design that is technically competent but uncalibrated — generic SaaS voice, default fonts, no awareness of what the design has to hold up under. With it, every command reads your answers before generating.

Reach for it when:
- You just installed /craft on a new project. First thing to run.
- The user invokes bare /craft or $craft and expects guidance.
- The project's positioning, audience, or stakes have shifted.
- Another /craft command stopped with "no design context found." Run start, then resume.

## How it works

The command writes PRODUCT.md and hands the user to the next foundation step:

- **PRODUCT.md** is the strategic file. Register, users, purpose, voice, references, anti-references, design principles, and Operating Conditions. Answers "who, what, why, and what's at stake." No colors, no fonts, no pixel values.
- **Next step** is explicit. Usually /craft document, then /craft features. If the project already has these files, recommend the most useful Judgment or Ship command instead.

The flow runs in four non-skippable phases:

1. **Scan.** Read README, package.json, components, brand assets, route structure. Form a register hypothesis (brand vs product) and a stakes hypothesis (low/medium/high-consequence) before asking anything.
2. **Confirm.** Quote the inference back. "From the routes and dependencies, this looks like a product surface for clinicians — match?" The user agrees, corrects, or refines.
3. **Interview.** Ask only what could not be inferred. Operating Conditions questions are non-negotiable. For voice and references, draft first and ask the user to correct it; do not make the user invent vocabulary from a blank page.
4. **Write.** PRODUCT.md lands at the project root. Start offers one clear next action: usually /craft document, then /craft features. Say yes unless there is a specific reason to hold off.

## Bare invocation behavior

When the user invokes /craft or $craft without a sub-command, do not respond with a menu of lenses. Run start.

Output shape:

```text
I'll start by calibrating /craft to this project.

What I can infer:
- [2-4 concrete observations from README, package.json, routes, UI, or filenames]

What I still need:
1. [highest-value question]
2. [Operating Conditions question]
3. [drafted voice/reference check if needed]

Next: answer these, and I'll write PRODUCT.md. After that the next step is /craft document.
```

If PRODUCT.md already exists, read it and say the next concrete command instead:
- DESIGN.md missing → "Next: /craft document"
- FEATURES.md missing → "Next: /craft features"
- all foundation files present → recommend exactly one of scope, ship, risk, signal, compose, responsive, or coach based on the user's stated concern and current project state.

## The wedge: Operating Conditions

Every PRODUCT.md carries the standard fields (register, users, voice, anti-references). What separates /craft start from generic design-context tools is a fourth section nobody else asks for:

- **Consequence when wrong.** What happens when a user makes a mistake here? Reversible inconvenience (navigation), lost work (unsaved state), real-world harm (medication, finance, vehicle controls), regulatory exposure (HIPAA, FDA, GDPR, accessibility law).
- **Environment.** Where, when, how is this used? Desk and focused, mobile and interrupted, kiosk in public, dim light, with gloves, in transit, under time pressure.
- **User skill level.** Who is at the controls? Beginner exploring, intermediate using daily, expert under pressure, mixed audience requiring fallbacks.

These three are linked, and they redirect every downstream command. /craft risk reads consequence to weight failure modes. /craft signal reads skill level to calibrate how much the UI has to telegraph. /craft ship reads all three to decide what is allowed to be janky and what is load-bearing. /craft compose reads environment to set density. Without these fields, every downstream command falls back to generic SaaS defaults.

## Interview script

**Phase 1 — Register**
"Looking at the code, I'd put this in the [brand | product] register. Sound right?"

**Phase 2 — Users and purpose**
"Who is the primary user, and are they already committed or still curious?"
"In one sentence, what is the user trying to accomplish here?"

**Phase 3 — Operating Conditions (non-skippable)**
"When a user makes a mistake on this surface, what happens? (Reversible / lost work / real-world harm / regulatory exposure)"
"Where and when is this used? (Desk + focused / mobile + interrupted / kiosk / other — be specific)"
"User skill level? (Beginner / intermediate / expert / mixed)"

**Phase 4 — Voice and references**
Do not make the user invent brand vocabulary from a blank page. That feels like homework.

Draft a point of view from the repo first, then ask for correction:

"Based on what I can see, I'd make this feel [draft voice: e.g. calm, direct, and exacting] — closer to [draft reference] than [draft anti-reference]. Is that direction right, or should it feel more like something else?"

If the repo doesn't give enough signal, ask in plain language with examples:

"What should this feel like to use? You can answer with products, places, objects, or vibes — for example: 'Linear but warmer,' 'a field notebook,' 'not a generic AI dashboard,' 'more clinic than fintech.'"

Then translate the answer into PRODUCT.md:
- **Voice**: 3-5 concrete words or short phrases from the repo and the user's correction.
- **References**: named products, brands, physical objects, books, films, places, or interfaces.
- **Anti-references**: named things or common AI defaults the product should avoid.

## PRODUCT.md template

```markdown
# PRODUCT.md

## Register
[brand | product]

## Users
[Primary user. State of mind. What they are trying to accomplish.]

## Purpose
[The one thing this product does for them. One sentence.]

## Operating Conditions

**Consequence when wrong:** [reversible inconvenience | lost work | real-world harm | regulatory exposure]
**Environment:** [where, when, how it is used — be specific]
**User skill level:** [beginner | intermediate | expert | mixed]

## Voice
[3-5 concrete words or short phrases, drafted from repo + user correction.]

## References
- [Named brands, products, printed objects, films]
- [...]

## Anti-references
- [Named things this should NOT feel like]
- [...]

## Design principles
[3-5 principles specific to this product, derived from everything above. Each principle is a one-liner the team can quote in code review.]

## Accessibility
[Required level: WCAG AA / AAA. Target devices and assistive tech. Any domain-specific requirements: HIPAA, Section 508, ADA, etc.]
```

## Pitfalls

- **Skipping it to "just try a command quickly."** Every other command will interview you mid-flight instead. Running start first is faster, not slower.
- **Generic Operating Conditions.** "Mixed audience" with no consequence specified produces generic output. The wedge questions force the specificity that calibrates every downstream command.
- **Treating PRODUCT.md as immutable.** The file is yours. Edit it freely; every command reads the current version.
- **Starting with vocabulary homework.** Draft the direction first. Users are better at correcting a concrete proposal than inventing a vocabulary from scratch.
- **Listing only adjectives for references.** Brands, products, printed objects: named, not described. "Braun radio designs," not "minimal and considered."
- **Skipping the scan phase.** Asking every question from scratch when the codebase already answers half of them wastes the user's time.

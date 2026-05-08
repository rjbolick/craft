# document

Loaded when /craft routes to the `document` sub-command.

Capture the visual system as DESIGN.md so every command stays on-brand.

## When to use it

Run /craft document once the project has enough of a visual system to document — colors, typography, at least a button and a card. /craft start invokes it automatically at the end of the start flow. You can also run it standalone when:

- The visual system has drifted from an older DESIGN.md
- Before a redesign, to capture the current state as reference
- A command nudges you ("DESIGN.md missing")

For projects with no code yet:

    /craft document --seed

Five strategic questions, written as a scaffold marked <!-- SEED -->. Re-run in scan mode once tokens exist.

## How it works

The command writes DESIGN.md at the project root in the Google Stitch format (https://stitch.withgoogle.com/docs/design-md/format/) — six sections, fixed order, fixed names. Other DESIGN.md-aware tools can parse this file because the format is interop-grade. Do not invent new top-level sections.

The flow runs in three phases:

1. **Read PRODUCT.md.** Operating Conditions calibrate every default. This is non-negotiable; if PRODUCT.md is missing, document offers to invoke /craft start first.
2. **Scan.** Find design assets in priority order: CSS custom properties, Tailwind config, CSS-in-JS themes, design tokens, component source, the global stylesheet, computed styles from rendered output if a browser is available. Auto-extract everything that can be auto-extracted.
3. **Confirm and ask.** One grouped question covers what needs creative input: a Creative North Star (single named metaphor for the whole system), descriptive color names ("Deep muted teal-navy," not "blue-800"), elevation philosophy, and component character.

Output is DESIGN.md plus a DESIGN.json sidecar for tooling.

## The six sections

Headers are fixed character-for-character. Other tools parse the file by these exact strings.

1. **Overview.** Creative North Star, philosophy, what the system optimizes for. Accessibility level lives here. Operating Conditions surface as principles ("Errors recoverable in two clicks or less" for high-consequence projects).
2. **Colors.** Palette with descriptive names alongside tokens. Accent strategy, usage rules. OKLCH for lightness consistency.
3. **Typography.** Families, scale, hierarchy. Fixed-rem for product surfaces, fluid clamp for marketing — the register from PRODUCT.md decides.
4. **Elevation.** Shadow philosophy, z-index scale, when depth is allowed. High-consequence projects often default to flat with elevation reserved for feedback.
5. **Components.** Per-component specs with all states explicit: default, hover, focus, active, disabled, loading, error, success. Touch-target minimums live here.
6. **Do's and Don'ts.** Named rules, imperative voice. "Tint neutrals toward the accent at low chroma." "Never use color as the only signal." Forceful on purpose. The file is for the AI, not human onboarding.

## How Operating Conditions calibrate output

This is the wedge. /craft document does not produce the same DESIGN.md regardless of context — Operating Conditions reshape every section.

- **High consequence + mixed skill** → Do's and Don'ts adds explicit rules about redundant signaling (color + icon + text for status), confirmation patterns on destructive actions, motion-as-feedback only. Components section requires every destructive action to specify its confirmation pattern.
- **Mobile + interrupted** → Components section sets touch-target minimums above 44px (typically 48-56px), tightens spacing scale, specifies thumb-zone constraints. Typography defaults to 16px minimum body, 18px for primary actions.
- **Regulatory exposure** → Overview's accessibility line names the specific compliance (WCAG AAA contrast, full keyboard operability, screen-reader testing as a Don't-skip). Do's and Don'ts adds rules referencing the regulation directly.
- **Expert + focused desk** → Density tightens, motion budgets relax, keyboard shortcuts and power-user patterns get permitted in Components. Typography drops to 14px body where appropriate.

A high-consequence medical interface and a low-consequence consumer landing page get genuinely different DESIGN.md files from the same scan, because Operating Conditions reshape what counts as a defensible default.

## Try it

    /craft document

Two minutes on a project with tokens defined. Scan finds the palette and type stack; confirm a Creative North Star from 2-3 options; pick descriptive color names; file lands.

    /craft document --seed

Five questions, five minutes. Output is a scaffold marked <!-- SEED -->. Re-run without the flag once tokens are implemented.

## Pitfalls

- **Running it without PRODUCT.md.** Document falls back to generic SaaS defaults. Start first, then document. Start invokes document automatically anyway.
- **Running it too early.** On a project with no implemented tokens, seed mode is correct. A fabricated full spec the code cannot back up is worse than no DESIGN.md.
- **Treating DESIGN.md as documentation for humans.** It is primarily for the AI. Every other /craft command reads it. Forceful imperative voice ("never," "always," named rules) is intentional.
- **Adding sections outside the six.** Layout content folds into Overview (philosophy) or Components (per-component). Motion content folds into Components (per-component behavior). New top-level sections break tool interop with the Google Stitch format.
- **Overwriting an existing DESIGN.md silently.** Document always confirms first. Rename the existing file or explicitly approve overwrite to start fresh.

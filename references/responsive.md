# responsive

Loaded when /craft routes to the `responsive` sub-command.

Does one design work across every context it actually needs to operate in?

## When to use it

Run /craft responsive when the design needs to hold up beyond the screen and input modality it was built on. Most designs are built on a 14-inch laptop with a trackpad, then deployed to phones, tablets, kiosks, and assistive tech. The result is a design that "works" in one place and fails everywhere else.

The command does not translate one context to another (that is a different kind of work). It audits whether a single design holds up across the full set of contexts named in PRODUCT.md and surfaces what breaks.

Reach for it:
- Before launch, when the design has only been validated on desktop
- After mobile usage data shows higher bounce or lower task completion than desktop
- When users on touch devices report misclicks or stuck flows
- When PRODUCT.md specifies mixed environment or multiple device classes
- When the team has been polishing one context at the expense of another

For aesthetic concerns at any single context, /craft coach. For affordance issues at any context, /craft signal. /craft responsive is specifically about cross-context behavior.

## How it works

The command walks six dimensions of cross-context behavior. Each generates findings; findings sort into three buckets.

The six dimensions:

1. **Layout integrity.** Does visual structure hold at every breakpoint named in PRODUCT.md? Overflow, broken grids, illegible columns, content stacking awkwardly, sticky elements covering primary actions, fixed headers eating viewport on mobile.
2. **Touch compatibility.** Do controls work with finger input where touch is named? Hit-target sizes, spacing between adjacent targets, hover-only affordances (tooltips, dropdowns, secondary actions), drag interactions that need precision input, double-tap-to-zoom side effects.
3. **Density calibration.** Is content density appropriate for each context? Too dense at mobile (cramped, illegible, requires zoom); too sparse at desktop (wasteful, hard to scan, low information per screen). Density is a per-context decision, not a single global choice.
4. **Content priority.** Is the right content visible at the right context? Critical info hidden behind disclosure on mobile; secondary content bloating desktop and burying the primary action; navigation collapsed where users need it most; settings prominent on the smallest screens where they matter least.
5. **Performance across network.** Does the experience hold on slow networks where named? Bundle weight on mobile, image strategy (responsive, lazy-loaded, modern formats), render performance on lower-powered devices, time-to-interactive on 3G.
6. **Modality and assistive tech.** Does the design work for keyboard-only navigation? Screen readers? Switch control? Voice control? When PRODUCT.md names these as user contexts, they get full review; when not, accessibility minimums still apply.

The three buckets:

- **Breaks** — fails outright in the named context. Layout broken, controls unusable, content unreachable, performance unacceptable. Must fix.
- **Compromises** — works but suboptimal. Functional but cramped, slow, or awkward. Calibrated decisions allowed if the team explicitly accepts them; ad-hoc compromises flagged as findings.
- **Solid** — adapts well. Survives the context with no caveats.

The flow runs in three phases:

1. **Read PRODUCT.md.** Operating Conditions name the contexts that matter. Without this, /craft responsive falls back to a generic "every device" check that grades the design against contexts it does not need to support, producing irrelevant findings.
2. **Inventory contexts to test.** Screen-size range, primary input modalities, network conditions, accessibility requirements. From PRODUCT.md plus any explicit overrides in the command call.
3. **Walk the six dimensions.** Each dimension checked against each context. Findings sort into Breaks / Compromises / Solid.

## How Operating Conditions reshape the review

The wedge. /craft responsive does not grade against the same contexts regardless of PRODUCT.md.

- **Environment "mobile + interrupted"** → mobile contexts get the strictest review. Touch compatibility, glanceability, performance under slow network all become load-bearing dimensions. Hover-only affordances are automatic Breaks. Compromises that work for a focused desktop user are Breaks for an interrupted mobile user.
- **Environment "desk + focused" only** → mobile review drops out entirely. The command grades desktop layout, density, keyboard nav, and screen reader compatibility — not mobile breakpoints. Naming the contexts that DON'T matter prevents the AI from flagging irrelevant findings.
- **Environment "mixed"** → both contexts reviewed equally; compromises in either are flagged. The hardest case: cannot optimize for one at the expense of the other.
- **Skill level "beginner" or "mixed"** → convention non-negotiable across contexts. Standard responsive patterns (hamburger menu on mobile, full nav on desktop) are required, not optional. Clever-but-novel responsive behavior is a finding because the user has no mental model for it.
- **Consequence "real-world harm" or "regulatory exposure"** → the critical-path flow must be flawless across every named context. Compromises in critical-path elements are Breaks. Edge contexts (older browsers, rare devices) may still be Compromises if explicitly accepted.

A high-consequence + mobile-first + mixed-skill review is long and strict, with most findings as Breaks. A low-consequence + desktop-only + expert review is short and forgiving, with mobile not assessed at all.

## Try it

    /craft responsive the patient-record form

Expected output shape:

    Operating Conditions: real-world harm + mixed skill + interrupted environment
    Contexts to test: mobile (primary), desktop (secondary), touch + mouse + keyboard, screen reader
    Network: 3G assumed for mobile

    BREAKS (fails outright in named context)

      [1] Patient name field uses a hover-revealed dropdown for selection
          Dimension: touch compatibility
          Context: mobile + touch
          Failure: hover state never triggers on touch; dropdown
          unreachable; the primary identification step is unusable
          on the primary device.
          Fix: convert to a click-to-open dropdown with explicit
          touch-friendly hit area. Hover state can persist on desktop
          as a secondary affordance.

      [2] Submit button covered by sticky footer below 600px viewport
          Dimension: layout integrity
          Context: mobile
          Failure: the action that commits the record is occluded
          on every common mobile screen size.
          Fix: footer becomes static below 600px; or submit moves
          above the fold with the footer below it.

      [3] Save action requires precision drag on a slider control
          Dimension: touch compatibility, modality
          Context: mobile + touch, keyboard-only
          Failure: drag precision unreliable on touch; slider
          unreachable via keyboard.
          Fix: replace with discrete buttons or stepper input that
          works across all input modalities.

    COMPROMISES (works but suboptimal)

      [4] Field labels positioned to the left of inputs
          Dimension: layout integrity, density
          Context: mobile
          Compromise: works at 320px but eats half the input width.
          Functional, but cramped enough to slow data entry.
          Fix: stack labels above inputs below 600px; left-align
          on desktop where horizontal space is plentiful.

      [5] Form validation shown in toast above keyboard
          Dimension: content priority
          Context: mobile + virtual keyboard
          Compromise: validation message appears but is partially
          covered by the on-screen keyboard during typing.
          Fix: inline validation below the field, visible above
          the keyboard fold.

    SOLID (adapts well)

      [6] Header navigation collapses to hamburger below 768px
      [7] Color contrast meets AAA on every named context
      [8] Tab order works for keyboard-only users
      [9] Screen reader correctly announces field labels and errors

    [...]

Hand findings to the implementing layer (or apply directly). /craft responsive diagnoses; the user implements.

## Pitfalls

- **Running it without PRODUCT.md.** The command grades against generic "every device" and produces irrelevant findings for projects that don't need full cross-context support. Start first.
- **Treating Compromises as immediate fixes.** Some compromises are calibrated decisions the team has made (e.g., desktop-first product where mobile is acceptable-janky). Compromises are flagged so the team can confirm them, not auto-fix them.
- **Skipping accessibility because PRODUCT.md doesn't name screen readers explicitly.** Accessibility minimums (WCAG AA contrast, keyboard operability, semantic markup) apply regardless. PRODUCT.md elevates them when stricter; it does not remove them.
- **Confusing /craft responsive with /craft signal.** Signal asks "does this UI tell users what to do?" Responsive asks "does this UI work in every named context?" A button can signal correctly on desktop and break on mobile. Both reviews matter; they answer different questions.
- **Auto-implementing fixes.** Same pattern as other Judgment commands. Diagnose; let the user route findings.
- **Treating "responsive" as just breakpoints.** The command checks input modalities, network conditions, and assistive tech in addition to screen sizes. A design can pass every breakpoint and still fail when used on touch with screen reader on a slow connection.

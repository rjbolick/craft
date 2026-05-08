# signal

Loaded when /craft routes to the `signal` sub-command.

Does the UI tell the user what they can do with it?

## When to use it

Run /craft signal when users hesitate, misclick, or miss features that exist. The command applies four affordance principles (Gibson/Norman, in plain English) to surface elements that signal the wrong thing, fail to signal at all, or signal too quietly to land.

Reach for it:
- After /craft risk identifies user-error failure modes — signal often prevents what risk catches
- When users keep asking "is this clickable?" or "where do I click to do X?"
- After a UX review flags discoverability or learnability issues
- Before shipping any UI to a beginner or mixed-skill audience

For aesthetic concerns, /craft coach. For Gestalt and hierarchy, /craft compose. /craft signal is about whether the UI announces its own function.

## How it works

The command runs through four affordance dimensions and reports findings in three categories.

The four dimensions (asked in this order):

1. **Affordance** — does the form match the function? Things that look clickable should be clickable; things that look static shouldn't be clickable. A decorative card that triggers navigation is an affordance failure. A "Submit" word styled identically to body copy is also an affordance failure.
2. **Signifier** — for non-obvious affordances, is there a perceivable cue? Drag handles, expandable rows, swipeable cards, hover-revealed menus, keyboard shortcuts. If the affordance exists but the cue does not, the feature might as well not exist.
3. **Mapping** — does the control's location, label, and visual treatment match what it controls? A delete button at the top of a record list could mean "delete the list" or "delete the selected item" or "delete the record being viewed." Mapping decides which.
4. **Feedback** — does the UI confirm the action happened, is happening, or failed? Click with no acknowledgment, async action with no loading state, success with no confirmation, error swallowed silently — all feedback failures.

Findings sort into three categories:

- **Wrong signal** — the UI signals something it shouldn't. A clickable element that looks decorative; a status indicator that looks like a button; disabled state indistinguishable from active.
- **Missing signal** — an affordance exists but has no perceivable cue. Drag-and-drop with no drag handle; right-click menus with no hint; keyboard shortcuts undocumented in-UI; required fields not marked.
- **Weak signal** — the cue is correct but too quiet. Loading state too subtle for an interrupted user to catch; focus ring too low-contrast for accessibility; tooltip on hover-only when the user is on touch.

The flow:

1. **Read PRODUCT.md.** Operating Conditions decide how strong signals need to be.
2. **Inventory.** List every interactive element, every state, every transition. Catalog what's clickable, typeable, draggable, swipeable, expandable, dismissable.
3. **Walk the four dimensions.** For each interactive element, run all four checks: affordance, signifier, mapping, feedback.
4. **Sort findings.** Wrong / missing / weak signal, with the specific fix for each.

## How Operating Conditions reshape the review

The wedge. /craft signal does not require the same signal strength regardless of context — Operating Conditions calibrate what "signaled enough" means.

- **Skill level "beginner" or "mixed"** → Convention is not enough; redundant signaling required. Color + icon + text for status. Explicit labels on icon-only buttons. No reliance on hover for non-decorative affordances.
- **Environment "interrupted" or "in transit"** → Signals must land at a glance. Hover-only affordances fail. Loading states need motion, not just opacity changes. Error states need persistent indicators, not toasts that disappear.
- **Consequence "real-world harm" or "regulatory exposure"** → Destructive and irreversible actions get explicit, redundant signaling. A delete button cannot rely on color alone (color-blind users); cannot rely on position alone (screen-reader users); cannot rely on icon alone (users who don't recognize the icon). All three.
- **Skill level "expert" + environment "desk + focused"** → Convention can carry more weight. Keyboard shortcuts can be discoverable rather than visible. Density can be higher. Power-user patterns are permitted.

A beginner-mixed + mobile + high-consequence project gets a long signal review with most findings in "missing" or "weak." An expert-focused + desk + low-consequence project gets a short review focused mostly on "wrong" — convention-violating elements.

## Try it

    /craft signal the dashboard

Expected output shape:

    Operating Conditions: lost work + mixed skill + desk + focused

    WRONG SIGNAL (the UI signals the wrong thing)

      [1] Status pills on row 3, 7, 12 styled identically to action buttons
          Affordance: looks clickable, isn't
          User assumption: "Click to change status"
          Fix: Drop button-style padding and border. Use a flatter
               text-only badge treatment. Reserve button styling
               exclusively for actions.

      [2] "View details" link styled as plain body text
          Affordance: looks static, is clickable
          User assumption: "Just descriptive text"
          Fix: Underline on hover at minimum; consider permanent
               link treatment in dense contexts.

    MISSING SIGNAL (affordance exists, no cue)

      [3] Drag-to-reorder on table rows, no drag handle visible
          Signifier: missing
          User assumption: "Rows are static"
          Fix: Add drag handle (⋮⋮) on row hover. Add helper text
               "Drag rows to reorder" above the table on first load.

      [4] Cmd+K opens search, no in-UI indicator
          Signifier: missing
          User assumption: feature does not exist
          Fix: Add ⌘K hint inside the search-icon button label.
               Surface in command-bar tour for first-run users.

    WEAK SIGNAL (cue exists but too quiet)

      [5] Disabled state on Submit button uses #e0e0e0 on #f5f5f5
          Signifier: present but invisible
          User assumption: button is active, click does nothing
          Fix: Strengthen contrast to WCAG AA (minimum 3:1 for
               disabled controls). Pair with cursor: not-allowed.
               Add tooltip explaining why disabled.

    [...]

Hand findings to /craft compose for layout fixes, or apply directly. /craft signal diagnoses; the user implements.

## Pitfalls

- **Running it without PRODUCT.md.** Signal strength requirements are Operating-Conditions-dependent. Start first.
- **Confusing /craft signal with /craft compose.** Compose is about Gestalt grouping, hierarchy, and IA. Signal is about whether individual elements announce their function. A grid can have perfect Gestalt grouping and still fail every signal check.
- **Confusing /craft signal with /craft risk.** Signal prevents the slip; risk catches it. A button that signals incorrectly (wrong signal) is a /craft signal finding. The same button causing user error is a /craft risk finding. The fix is often the same; the framing is different.
- **Treating "looks like a button" as automatic affordance success.** Every "Click here to learn more" card on a marketing site signals "button" but only one is the actual CTA. Wrong signals on decorative elements steal attention from the right ones.
- **Ignoring touch-only environments.** Hover affordances fail entirely on touch. If PRODUCT.md says mobile or kiosk, all hover-only signaling counts as missing.
- **Auto-implementing fixes.** Same pattern as /craft risk. Diagnose; let the user route findings.

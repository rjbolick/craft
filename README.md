# /craft — Design judgment for AI-assisted work

A skill by Ryan Bolick. Ten coordinated commands for intentional design that holds up under real users, real environments, and real consequences.

Built for designers, founders, engineers, and educators shipping considered, intentional design at velocity.

## What /craft is

/craft is built around three questions: does this design hold up under real users? Real environments? Real consequences?

The skill organizes around three categories:

- **Foundation** writes the context files every command reads: PRODUCT.md (strategy plus Operating Conditions), DESIGN.md (visual system in Google Stitch format), FEATURES.md (canonical inventory at five description levels).
- **Judgment** reviews UI through specific lenses: pedagogical critique, FMEA-style failure-mode analysis, affordance and discoverability, applied composition fundamentals, cross-context behavior.
- **Ship** triages quality and defends against overbuilding: scope cuts work that doesn't earn its place; ship calibrates what's load-bearing versus what can be acceptable janky.

PRODUCT.md captures three fields most design tools never ask about:

- **Consequence when wrong** — reversible inconvenience, lost work, real-world harm, or regulatory exposure
- **Environment** — desk and focused, mobile and interrupted, kiosk, in transit, with gloves
- **User skill level** — beginner, intermediate, expert, or mixed

Every /craft command reads these and recalibrates output to the actual stakes.

## Install

Recommended:

```bash
npx skills add rjbolick/craft
```

The `skills` CLI discovers the root `SKILL.md` and installs the skill into the right directory for supported agents.

Manual install:

| Agent | Install path | Invoke |
|-------|--------------|--------|
| Codex | `~/.codex/skills/craft` | `$craft` |
| Claude Code | `~/.claude/skills/craft` | `/craft` |
| Other Agent Skills-compatible tools | The tool's skills directory | Tool-specific |

Clone directly if you prefer manual install:

```bash
git clone https://github.com/rjbolick/craft.git ~/.codex/skills/craft
git clone https://github.com/rjbolick/craft.git ~/.claude/skills/craft
```

Restart or open a fresh agent session after manual install so the skill list reloads.

For Claude.ai, zip this folder and upload it through Settings -> Features -> Skills.

## First run

After install, on a new project:

| Step | Codex | Claude Code | Result |
|------|-------|-------------|--------|
| 1 | `$craft start` | `/craft start` | Writes PRODUCT.md with strategy and Operating Conditions |
| 2 | `$craft document` | `/craft document` | Writes DESIGN.md with the visual system |
| 3 | `$craft features` | `/craft features` | Writes FEATURES.md with the feature inventory |

Then invoke any specific command, or use freeform mode (`$craft <request>` or `/craft <request>`) to let the auto-router pick.

## The ten commands

| Command | What it does |
|---------|--------------|
| `start` | Writes PRODUCT.md (strategy + Operating Conditions) |
| `document` | Writes DESIGN.md (visual spec, Google Stitch format) |
| `features` | Writes and reconciles FEATURES.md (multi-level feature inventory) |
| `coach` | Pedagogical critique — teaches as it evaluates |
| `risk` | FMEA-style failure mode review (severity × probability × detectability) |
| `signal` | Affordance and discoverability review |
| `compose` | Applied UI fundamentals (Gestalt, hierarchy, IA, composition) |
| `responsive` | Cross-context behavior (screens, modalities, networks, assistive tech) |
| `scope` | Defends against overbuilding (cut what doesn't earn its place) |
| `ship` | Quality triage (load-bearing / acceptable janky / stop polishing) |

Recommended pre-launch workflow:

1. start → document → features (Foundation)
2. scope (cut what shouldn't exist)
3. risk, signal, compose, responsive (review what remains)
4. coach (pedagogical pass)
5. ship (final triage)

Skipping step 2 means later steps grade work that should have been cut.

## Structure

```text
craft/
├── README.md
├── LICENSE
├── SKILL.md
└── references/
    ├── start.md
    ├── document.md
    ├── features.md
    ├── coach.md
    ├── risk.md
    ├── signal.md
    ├── compose.md
    ├── responsive.md
    ├── scope.md
    └── ship.md
```

Single-skill repo: SKILL.md at the root, `references/` alongside. Progressive disclosure keeps the entry skill short and decisive about routing; per-command details load only when invoked.

## The thinking

The decisions behind /craft, organized into themes.

### Stakes-aware design

/craft grades designs against the actual stakes of the project, not against universal best practices. Every command reads PRODUCT.md's Operating Conditions and recalibrates. /craft risk uses three-axis FMEA scoring (severity × probability × detectability) because Operating Conditions inform all three independently. /craft ship decides what's load-bearing based on stakes, not heuristics. /craft signal requires stronger affordance for beginner audiences than expert ones. The same code gets different reviews under different stakes.

### Restraint as a value

AIs tend to overbuild. Given any request, the model adds completeness features, edge-case handling, settings nobody asked for, and depth that's not warranted by the actual user problem. Two commands exist specifically to push back:

- `/craft scope` runs early in the workflow and triages every feature, screen, and flow step against three lenses (user-problem fit, depth calibration, flow simplicity). It explicitly names AI overbuild patterns: settings pages with 30+ options when 5 cover 95% of use; form validation with 12 rules when 3 catch all real errors; admin tools before there are customers. Findings sort into Cut, Simplify, or Keep.
- `/craft ship` triages quality with a three-bucket framework: load-bearing (must be solid), acceptable janky (defer), stop polishing (wasteful effort). The third bucket is the founder-honest move — naming where time is being spent on items that won't move outcomes.

The skill itself practices restraint. Ten commands plus a router, not twenty-three. Five status values would be too many; three (shipped / planned / deprecated) catches everything that matters. The discipline applies to the skill's own design.

### Frameworks over taste

The principles in /craft are universal. The aesthetic choices made on top of them are the user's. The skill is built so workshop attendees, founders, and students can apply the frameworks to their own work — not so they can adopt one designer's taste.

The job of /craft is to make design judgment transmissible — not to make every project look the same.

### Reconcile, don't regenerate

Two commands rebuild context that already exists rather than starting from blank pages:

- `/craft start` uses a register hypothesis pattern. It scans the codebase first, infers what kind of project it is (brand vs product, low-stakes vs high-consequence), and asks only what couldn't be inferred. The interview is shorter, sharper, and more accurate than a from-scratch questionnaire.
- `/craft features` runs in diff mode on every invocation. It scans the codebase, reads existing FEATURES.md, and surfaces three categories of drift (new features, stale status, orphans) rather than regenerating the file. Each invocation is a reconciliation pass that catches drift cheaply.

The pattern across commands: AI drafts from available context, user confirms or refines. AI is competent at drafting from code plus PRODUCT.md; users are good at catching what AI got wrong. Asking users to fill in blank fields defeats the point of using an AI tool.

### Self-contained

/craft doesn't require any other skill, plugin, or agent-specific extension. The ten commands work together; nothing depends on anything outside the skill. The frameworks stand on their own.

This matters because skills get evaluated on their own merits, not by their compatibility with a specific tool. Someone reading the SKILL.md files should learn how /craft thinks about design — not be sent elsewhere to fill in gaps.

## About the Author

Ryan Bolick teaches at Duke (design engineering, building with AI, AI entrepreneurship — to both technical and non-technical students and professionals), builds companies (most recently Byline, an authorship-transparency platform for education), ships tools that codify how he thinks (this is one of them), and advises teams shipping AI-augmented products.

His background spans industrial design (MS), biomedical engineering (BS), and founding-team experience at two Y Combinator companies (joined pre-seed in both). /craft is the synthesis — design frameworks from industrial and clinical practice, applied to digital products, refined for the calendar pressure of 0→1 work.

Find him at [ryanbolick.com](https://ryanbolick.com).

## Design philosophy

Ryan believes design should serve real user problems. The best design is honest about what something is, what it does, and what happens when it fails.

His approach is shaped by working in both high-risk and high-velocity environments. He has a background in industrial design and biomedical engineering — disciplines that treat user error, environment, and skill level as first-class design inputs. Operating Conditions in /craft reflect how engineering disciplines have approached design problems for decades, applied here to digital products.

Ryan has been part of the founding team for two Y Combinator companies, joining pre-seed in both. The Ship category exists because perfect is the enemy of shipped, and most quality tools tell you to perfect everything. Calibrating what to make solid and what to leave janky is a real skill — and one most quality reviews never name.

He teaches design engineering, building with AI, and AI entrepreneurship at Duke to both technical and non-technical students and professionals. Most engineers don't lack the will to build good UI; they lack the framework. /craft codifies the framework so design judgment can be transmitted, not just demonstrated.

What he values: first principles, intentional solutions, attention to detail, honesty in copy, frameworks that travel.

More of his work and aesthetic at [ryanbolick.com](https://ryanbolick.com).

## Contributing

Issues welcome at [github.com/rjbolick/craft](https://github.com/rjbolick/craft). The skill is intended to evolve based on real workshop and project use.

## License

Apache 2.0.

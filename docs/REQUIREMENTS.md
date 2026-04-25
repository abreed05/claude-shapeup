# Shape Up Plugin — Requirements

> Requirements spec produced via `/sc:brainstorm`. No architecture or implementation decisions here — those belong to `/sc:design` and `/sc:implement`.

## 1. Goal

Build a Claude Code plugin (`shapeup-skill`) that implements Ryan Singer / Basecamp's **Shape Up** methodology as a family of opinionated skills. The first and primary skill is a **shaping assistant** that turns raw feature ideas into properly shaped pitches per Shape Up discipline.

Source of truth for the methodology: https://basecamp.com/shapeup

## 2. Plugin Family — Scope

Namespace: `shapeup:*`. This pass builds only `shapeup:shape` and the plugin scaffolding; the other skills are reserved slots only.

| Skill | Status | Purpose |
|---|---|---|
| `shapeup:shape` | **In scope (build)** | Shaping assistant — produces shaped pitches |
| `shapeup:bet` | Reserved slot | Betting-table facilitator |
| `shapeup:hill-chart` | Reserved slot | Build-phase progress tracking |
| `shapeup:scope-hammer` | Reserved slot | Mid-cycle scope cutting |
| `shapeup:cycle-plan` | Reserved slot | 6-week cycle planning |
| `shapeup:retro` | Reserved slot | Post-cycle retrospective |

## 3. `shapeup:shape` — Shaping Assistant

### 3.1 Users
Solo developers or any team member who installs the plugin. No team/tenancy assumptions.

### 3.2 Invocation
- **Explicit:** `/shapeup:shape [slug]` — slug is optional. If missing, the skill prompts with a smart default auto-slugified from the problem statement once it's been established.
- **Auto-invoke:** the skill's `description` field must include triggers so Claude activates it when a user expresses intent to pitch, shape, or frame a new feature/initiative. Examples of triggering phrases: "I want to build X", "let's pitch Y", "new feature idea", "shape this up".

### 3.3 Interaction Model — Adaptive
On first turn, the skill asks explicitly: **"Do you have a rough draft already, or are we starting from scratch?"** Based on the answer it picks one of two sub-modes:

- **Dialogue mode** — walks the user through sections in order (problem → appetite → solution → rabbit holes → no-gos), one at a time, refusing to advance until each passes the opinionated rules. Used when starting from scratch.
- **Draft-critique mode** — the user dumps a rough idea; skill produces a first-pass draft pitch, then applies opinionated critique with concrete pushback. Used when rough material already exists.

Explicit triage avoids brittle heuristics on input length — a 500-word dump can still be vague, and a one-line idea can still be focused enough to draft from.

### 3.4 Output Layout

Per-pitch directory (section files chosen to keep AI context small — a skill only loads the section it's working on; `pitch.md` exists for humans/stakeholders):

```
pitches/<slug>/
├── pitch.md           # one-page summary linking/embedding sections — shareable artifact
├── problem.md         # concrete user scenario, raw quote or situation
├── appetite.md        # small batch / big batch / justified exception
├── solution.md        # fat-marker solution + mermaid breadboard
├── rabbit-holes.md    # identified risks and mitigations
└── no-gos.md          # explicit scope cuts
```

- **Pitches root:** `./pitches/` at project root by default; overridable via `plugin.json` config for projects that already use the name or want a different location.
- **Slug derivation:** slug may be passed as an arg (`/shapeup:shape <slug>`); if absent, the skill prompts with a smart default auto-slugified from the problem statement once established.
- **Section files vs summary:** skills read/write the five section files; `pitch.md` is generated/refreshed from them as the final shareable artifact.
- **Fat-marker sketches** are rendered as **mermaid diagrams** inside `solution.md`, showing places, affordances, and connection lines (breadboard style).

### 3.5 Opinionated Rules — Hard Stops

When a rule is violated the skill:
1. Names the violation
2. Cites the Shape Up principle it violates
3. Offers **2–3 concrete suggestions** for how to fix it
4. **Does not proceed** until resolved (hard stop)

Priority order (highest first):

| # | Rule | Violation examples |
|---|---|---|
| 1 | **Appetite must be declared** — this is the primary Shape Up discipline | "no appetite", "a few weeks", "some time", missing section |
| 2 | **Non-standard appetite requires written justification** | Appetites other than small batch (1–2 wks) or big batch (6 wks) must include a justification paragraph stored in `appetite.md` — a dismissible warning isn't sufficient; the betting table needs to see the reasoning |
| 3 | **No solutioning in the problem statement** | "we need a dashboard" stated as a problem |
| 4 | **Problem must be concrete** | Abstract framing with no user scenario, raw customer quote, or specific situation |
| 5 | **Solution must stay at fat-marker level** | Wireframes, field-level specs, specific API designs, pixel details |
| 6 | **Rabbit-holes section is mandatory** | "none" or empty |
| 7 | **No-gos section is mandatory** | "none" or empty |

### 3.6 Session Resumption
Every invocation is self-contained; the skill does not rely on conversation memory across sessions.

On invoke with `/shapeup:shape <slug>`:
- If `pitches/<slug>/` does not exist → new pitch; start at triage.
- If it exists → scan files, determine which sections are present and which pass opinionated rules, then resume at the first missing or violating section.

## 4. Shared References

A `references/` directory at plugin root holds Shape Up concept explainers. Skills load these on demand (via `Read`) rather than inlining concepts. This keeps individual skill files lean and gives the family a consistent vocabulary.

```
references/
├── shape-up-overview.md    # cycle structure, philosophy
├── appetite.md             # small batch vs big batch, why time-boxed
├── pitches.md              # anatomy of a pitch, section definitions
├── breadboards.md          # fat-marker principle + mermaid examples
├── rabbit-holes.md         # common patterns
├── circuit-breaker.md      # ship discipline
├── hill-chart.md           # used later by shapeup:hill-chart
├── betting-table.md        # used later by shapeup:bet
└── scope-hammering.md      # used later by shapeup:scope-hammer
```

## 5. Plugin Structure

```
shapeup-skill/
├── .claude-plugin/
│   └── plugin.json         # plugin metadata (name, version, author, etc.)
├── README.md
├── skills/                 # skills are auto-discovered, no listing needed
│   ├── shape/              # fully implemented
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── pitch.md
│   │       ├── problem.md
│   │       ├── appetite.md
│   │       ├── solution.md
│   │       ├── rabbit-holes.md
│   │       └── no-gos.md
│   ├── bet/SKILL.md          # stub: "Planned — not yet implemented."
│   ├── hill-chart/SKILL.md   # stub
│   ├── scope-hammer/SKILL.md # stub
│   ├── cycle-plan/SKILL.md   # stub
│   └── retro/SKILL.md        # stub
├── references/             # shared concept explainers (§4)
└── REQUIREMENTS.md         # this file
```

**Reserved-slot skills:** each has a stub `SKILL.md` (auto-discovered) with a description of the planned purpose; activation returns "This skill is planned but not yet implemented. See the plugin README for status." Gives users visibility into the family roadmap and a clear failure mode when they try to invoke something that isn't built yet.

**Pitches-root override:** since Claude Code's `plugin.json` is a fixed metadata schema and doesn't host arbitrary user config, the override lives in a project-level config file (see DESIGN.md §5 for details).

## 6. Non-Functional Requirements

- **Markdown-first output**, no Confluence integration in this pass.
- **Context efficiency** — skill loads only the section file currently being worked on; references are loaded on demand.
- **Resumability without conversation memory** — state must live entirely on disk under `pitches/<slug>/`.
- **Idiomatic markdown** — write normal markdown (headings, lists, tables, code, mermaid). Don't pre-constrain for Confluence; a future export skill will handle conversion.
- **Opinionated descriptions** — skill SKILL.md descriptions should make the auto-invoke triggers unambiguous.

## 7. User Stories / Acceptance Criteria

**US-1 — Pitch a new idea from scratch**
*Given* I have a rough feature idea
*When* I say "I want to pitch X" or invoke `/shapeup:shape`
*Then* the skill triages my raw material, picks dialogue or draft-critique mode, and produces a complete `pitches/<slug>/` directory where every section passes the opinionated rules.

**US-2 — Resume an in-progress pitch**
*Given* `pitches/<slug>/` exists but is incomplete
*When* I invoke `/shapeup:shape <slug>`
*Then* the skill reads the existing files, identifies missing or violating sections, and continues from the first gap.

**US-3 — Fix a caught violation**
*Given* my pitch violates a rule (e.g. vague appetite)
*When* the skill rejects it
*Then* I receive the rule name, the Shape Up principle it violates, and 2–3 concrete suggestions; the skill does not advance until I resolve it.

**US-4 — Breadboard my solution**
*Given* I'm writing the solution section
*When* the skill prompts for a fat-marker sketch
*Then* it generates a mermaid breadboard showing places / affordances / connections — not wireframes.

**US-5 — Install and orient**
*Given* I've just installed `shapeup-skill`
*When* I read the plugin README
*Then* I can see which skills are available, which are reserved, and how to invoke the shaping assistant.

## 8. Decisions

All resolved — recorded here for design-pass reference:

| # | Question | Decision |
|---|---|---|
| 1 | Slug derivation | Optional arg to `/shapeup:shape`; if missing, skill prompts with auto-slugified default from the problem statement |
| 2 | Pitches root | `./pitches/` at project root; overridable via a project-level config file (see DESIGN.md §5) |
| 3 | Top-level summary | Generate `pitches/<slug>/pitch.md` as the human-shareable one-page summary |
| 4 | Triage signal | Ask explicitly on first turn: "Rough draft already, or starting from scratch?" |
| 5 | Reserved-slot skills | Stub `SKILL.md` in each, listed in `plugin.json`, activation returns "planned — not yet implemented" |
| 6 | Non-standard appetite | Require **written justification** stored in `appetite.md` — not a dismissible warning |
| 7 | Confluence-friendly markdown | Defer — write idiomatic markdown now, build converter later |

## 9. Explicitly Out of Scope

- Implementing the other family skills (`bet`, `hill-chart`, `scope-hammer`, `cycle-plan`, `retro`). Reserved slots only.
- Confluence integration. Future pass.
- Architecture diagrams, detailed file schemas, trigger-phrase wording — these belong to `/sc:design`.
- Any code. This is a requirements document only.

## 10. Next Steps

1. Run `/sc:design` to produce the plugin's detailed design: SKILL.md trigger language, rule-enforcement implementation, template file contents, `plugin.json` shape, reference doc outlines.
2. Run `/sc:implement` (or `/sc:workflow`) to build it.

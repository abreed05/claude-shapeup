# shapeup

Shape Up methodology ([Basecamp](https://basecamp.com/shapeup), Ryan Singer) as a family of opinionated Claude Code skills. The first skill turns raw feature ideas into properly shaped pitches — problem, appetite, solution, rabbit holes, no-gos — and hard-stops when you try to skip the discipline.

## Install

This is a Claude Code plugin. Install it with two commands:

```
claude plugin marketplace add abreed05/claude-shapeup
claude plugin install shapeup
```

The first command registers this repo as a marketplace source. The second installs the plugin. After install, restart Claude Code and the skills appear under the `shapeup:` namespace and can be invoked via slash command (e.g. `/shapeup:shape`) or auto-triggered when you describe a matching task.

## Skills in this family

| Skill | Status | Purpose |
|---|---|---|
| `shapeup:shape` | **Ready** | Shaping assistant — produces a shaped pitch |
| `shapeup:bet` | Planned | Betting-table facilitator for cycle selection |
| `shapeup:hill-chart` | Planned | Build-phase progress tracking |
| `shapeup:scope-hammer` | Planned | Mid-cycle scope cutting |
| `shapeup:cycle-plan` | Planned | 6-week cycle planning |
| `shapeup:retro` | Planned | Post-cycle retrospective |

## Quickstart

Invoke the shaping skill explicitly:

```
/shapeup:shape magic-inbox
```

…or just describe the task in plain language ("I want to pitch a new feature called magic-inbox") and Claude will load the skill on its own.

The skill asks whether you have a rough draft already or are starting from scratch, then walks you through the five pitch sections. It writes each section as a standalone markdown file under `./pitches/<slug>/` and generates a `pitch.md` summary once every section passes discipline.

## Configuration

By default, pitches are written to `./pitches/` in your project. Override the location by creating `./.shapeup.json` at the project root:

```json
{
  "pitchesRoot": "docs/shape-up/pitches"
}
```

## Philosophy

Shape Up is opinionated, and so is this skill. It **hard-stops** when you violate the discipline — vague appetites, solutioning in the problem statement, wireframes where a fat-marker sketch belongs, empty rabbit-holes or no-gos sections. You get the violation name, the Shape Up principle it breaks, and two or three concrete fixes. It does not soften the rules to be agreeable.

If you want a yes-man, this isn't it. That's the point.

## References

- Source: https://basecamp.com/shapeup
- Shared concept library: [`references/`](./references/) — short explainers for appetite, breadboards, rabbit holes, circuit breaker, hill chart, betting table, and more. The skills load these on demand rather than inlining the concepts.

## Contributing

Design docs live in [`docs/`](./docs/):

- [`docs/REQUIREMENTS.md`](./docs/REQUIREMENTS.md) — what this plugin does and why
- [`docs/DESIGN.md`](./docs/DESIGN.md) — how it's built

Read both before opening a PR that changes skill behavior or adds a new skill.

## License

MIT

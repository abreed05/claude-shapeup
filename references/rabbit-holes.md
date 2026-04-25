# Rabbit Holes

**Rabbit holes** are places in the work where the team could get stuck — unknowns, integration hazards, subtle complexity that could consume the cycle if it bites.

## Why they belong in the pitch

Shaping is where we find rabbit holes **in advance** and either plan around them or accept the risk consciously. If we skip this, the building team hits them mid-cycle with no context, and the circuit breaker fires on a bet we could have shaped better.

"None" is almost never true. If a pitch claims no rabbit holes, either:

- It's trivial (and probably didn't need shaping), or
- The shaper didn't look hard enough.

## How to find them

Ask these questions of every pitch:

1. **What's the hardest-to-estimate part?** The thing where "a few days" could turn into "two weeks."
2. **What would a senior engineer flag?** If you showed this pitch to someone with scar tissue, what would they point at?
3. **What external dependencies are on the critical path?** APIs, permissions, data access, other teams' work.
4. **What assumptions are we making?** List the ones that, if wrong, would force a scope cut or a drop.
5. **What has surprised us before?** Similar work that looked simple and wasn't.

## How to write them

Each entry:

- **Name** — one-line title you could say out loud.
- **Risk** — what goes wrong, and how it stalls the cycle.
- **Mitigation** — either a concrete containment plan, or the signal that will tell us to cut and move on.

## Common patterns

- **"The integration" rabbit holes.** Anything touching an external API you don't control.
- **"The data migration" rabbit holes.** Anything that assumes the existing data is clean.
- **"The edge case" rabbit holes.** Anything where "handle X gracefully" hides a major branch.
- **"The permissioning" rabbit holes.** Anything that needs new access controls, roles, or auth flows.
- **"The coordination" rabbit holes.** Anything that needs another team to also ship something.

If your rabbit holes section has none of these patterns, double-check — one of them is probably hiding.

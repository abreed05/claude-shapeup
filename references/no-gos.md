# No-Gos

**No-gos are explicit scope cuts** — the things we have deliberately decided **not** to include in this bet. They belong in every pitch.

## Why they belong in the pitch

A pitch without no-gos is a project, not a bet. "Everything reasonable is in scope" means the building team has to guess at the boundary, and the betting table can't tell the shape of the wager. No-gos close off the shape.

## What a no-go looks like

A no-go is a concrete thing someone might reasonably expect to be included, written as a one-liner plus the reason it's out:

> - **No mobile layout** — desktop-only for this cycle; mobile is a separate bet if the usage warrants it.
> - **No admin roles for the new feature** — creators are the only user type v1; admin access can be a follow-up.
> - **No migration of existing records** — new records only; migration is a distinct project with its own shape.

Each no-go is **an adjacent thing someone would likely ask about**. That's what makes it useful — it pre-answers the question "why isn't X here?"

## What a no-go is NOT

- Not a wishlist of out-of-scope ideas you never considered.
- Not a disclaimer ("this is v1").
- Not a backlog placeholder.

The test: could a reasonable observer have expected this to be in the bet? If yes, it's a no-go. If no, it's noise.

## How to find them

Walk through the adjacent surfaces of the feature:

- The next-most-obvious user type (admins? external? mobile? API?)
- The next-most-obvious life-cycle stage (editing? deleting? archiving? migrating?)
- The next-most-obvious integration (email? Slack? export? import?)
- The next-most-obvious non-functional dimension (i18n? accessibility? performance at scale?)

For each, ask: are we handling this, or cutting it? Cutting goes in no-gos.

## Minimum

At least one explicit no-go. Usually three to five is the sweet spot for a big-batch pitch.

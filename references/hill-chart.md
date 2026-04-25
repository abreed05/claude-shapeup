# Hill Chart

*Used by the (planned) `shapeup:hill-chart` skill for build-phase progress tracking.*

The hill chart is Shape Up's alternative to burndown charts. It visualizes progress on each **scope** of work as a position on an S-curve.

## The two halves

- **Uphill side — figuring it out.** You know the scope exists but you haven't solved it yet. Unknowns remain. Progress here is **discovering**, not **producing**.
- **Downhill side — making it happen.** The approach is clear; you're executing. Progress here is **producing**, and it's much more predictable.

Each scope starts at the bottom of the uphill side, climbs to the top ("I see the solution"), then rolls down to done.

## Why it's better than burndown

Burndown charts conflate two very different kinds of work. A scope that's "50% done" on a burndown could mean:

- Halfway through predictable work (downhill), or
- Halfway through figuring out the unknowns (uphill).

These have wildly different remaining-time distributions. The hill chart makes the distinction visible so the team can see which scopes are still risky.

## The typical surprise

Scopes often sit on the uphill side longer than people expect. That's normal — the unknown IS the work. The chart is useful because it shows this honestly instead of hiding it in percent-complete numbers.

## Common patterns to watch

- **A scope stuck on the uphill for weeks.** Likely a rabbit hole the team didn't see during shaping. Candidate for scope-hammering.
- **A scope jumping straight to the top without climbing.** Probably wishful thinking — someone declared it "figured out" without actually solving it.
- **Many scopes simultaneously on the uphill side late in the cycle.** The cycle is in trouble. Cut scopes; don't extend time.

## Connection to shaping

Good shaping reduces the uphill time. Rabbit-holes identified during shaping become known-unknowns instead of surprise-unknowns, which shortens the climb. Pitches with weak rabbit-hole sections produce scopes that camp on the uphill.

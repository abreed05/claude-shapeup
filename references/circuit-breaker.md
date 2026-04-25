# Circuit Breaker

**The circuit breaker is Shape Up's shipping discipline.** If a bet doesn't ship within its cycle, it doesn't ship. No extensions. No carry-over. No "we're almost done, just one more week."

## Why it exists

Without a hard cap, cycles drift. Drift invalidates the bet — you agreed to spend X time on the problem, and now you're spending X+Y. The appetite stops being real. The betting table stops being able to make confident bets. Over time, "everything is important, extend everything" becomes the culture.

The circuit breaker protects the appetite. When the cycle ends, the bet either shipped or it's dropped.

## What dropping actually looks like

Dropped doesn't mean deleted forever. It means:

- The current work is set down.
- The problem goes back into the shaping pool if it's still worth solving.
- A new pitch can be shaped in the next cool-down with different assumptions.
- The code — if any is salvageable — can be reused by a future bet, but there's no pressure to preserve it.

Dropping is **information**. It tells the shaping team that the pitch was mis-shaped, the appetite was wrong, or the problem was harder than it looked. That's valuable even if nothing shipped.

## Why teams resist it

- "We're 90% done." (No, you're not. You're 90% done with a pitch that didn't fit the appetite. The remaining 10% is the rabbit hole you didn't see.)
- "The work would be wasted." (The information isn't wasted. The shape was wrong; that's worth knowing.)
- "Leadership won't accept it." (Leadership bought the cycle structure for a reason. This is where that structure pays off.)

## When it's OK to extend

Practically: when there's genuine agreement at the betting table that the pitch was mis-shaped AND the work done is worth preserving AND the extension is shape-like (a new bet on the remaining work), not drift-like (just keep going).

This is rare. Treat it as rare.

## Connection to scope-hammering

The circuit breaker is the reason scope-hammering exists. Mid-cycle, when the team realizes the bet won't fit, they **cut scope** (not extend time) so something shippable lands before the cap. See `references/scope-hammering.md`.

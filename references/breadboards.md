# Breadboards / Fat-Marker Solutions

Shape Up uses two visual languages during shaping: **breadboards** (for flow and affordances) and **fat-marker sketches** (for layout). This skill uses mermaid diagrams as stylized breadboards.

## What a breadboard captures

- **Places** — screens, views, states the user can be in.
- **Affordances** — actions available in each place (named generically, not labeled with specific UI copy).
- **Connections** — edges between places, labeled with the affordance that triggers the transition.

That's it. No layout. No typography. No color. No field-level detail.

## What a breadboard does NOT capture

- Wireframes (layout, grids, spacing).
- Copy (button labels, headings, placeholder text).
- Field specifications (types, validation rules).
- API endpoints or data shapes.
- Component structure (Navbar, Sidebar, Footer).

If any of those appear in your solution section, you're past shaping. Cut them back.

## Mermaid example (a reasonable breadboard)

```mermaid
flowchart LR
    inbox[Inbox] -->|open message| message[Message view]
    message -->|reply| composer[Composer]
    composer -->|send| inbox
    message -->|archive| archive[Archive]
```

Four places, four affordances, one cycle. Readable in five seconds. That's a fat-marker solution.

## Mermaid example (too detailed — don't do this)

```mermaid
flowchart TB
    A[Login page with OAuth button and email field] --> B[Email validation spinner]
    B -->|valid| C[2FA code entry 6 digits]
    B -->|invalid| D[Error tooltip red text]
    C --> E[Dashboard with KPI cards A B C]
    E --> F[Settings modal with tabs Account Billing Team]
    F --> G[Team invite form with role dropdown]
```

This is a wireframe described in nodes. Twelve nodes, specific copy, field counts, UI elements. Cut it back to 4–5 nodes at the affordance level.

## The heuristic

If your breadboard has more than about 8 nodes, or if any node label names a UI element type (button, modal, card, tab), you've slipped into wireframing. Collapse and redraw.

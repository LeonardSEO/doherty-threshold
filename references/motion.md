# Purposeful, accessible web motion

Read when implementing or reviewing animation. Use the project's established motion tokens and library first. These mechanics target the web; preserve platform-native conventions if adapting the broader interaction principles elsewhere.

## Choose the feedback before the API

Establish the action, whether it can be reversed, who initiated it, its spatial source, and the information the change must communicate. Choose the lowest intensity that conveys that information. Static state changes remain valid feedback.

| Interaction | Starting point | Typical duration |
|---|---|---|
| Press or hover | Subtle active state, optional small scale/opacity change | 80–120 ms |
| Toggle, checkbox, selection | State updates immediately; thumb/check/pill settles | 120–200 ms |
| Copy, save, submit | Immediate pending acknowledgment; confirmed text/icon after outcome | 120–200 ms for visual transition |
| Menu, popover, dialog | Short anchored fade/translation if useful | 160–280 ms |
| Dismissal | Reverse the meaningful entry direction, usually faster | 100–200 ms |
| Add, remove, reorder | Keep item identity and explain the new position | 180–300 ms |
| Skeleton to content | Direct swap or brief opacity transition | 0–150 ms; never gate usability |
| Drag | Follow current input; optional small settle on release | Every frame; settle about 150–300 ms |

These are starting ranges, not mandatory waits. Do not slow down a frequent interaction to fit a prescribed duration. Hero choreography belongs only in a task that calls for it; do not add it to ordinary business flows.

Use ease-out for settling entrances, ease-in for departures when appropriate, and stable project tokens for consistency. Linear motion fits continuous progress or rotation; springs fit gestures when useful. Avoid exaggerated bounce, error shaking, unsolicited sound/haptics, or decorative movement on every click.

Open and close should preserve spatial relationships. Respect RTL direction and where the control actually originates. A meaningful exit can end sooner or be skipped when an interruption makes the next state more useful.

## Tool selection

| Need | First reasonable tool |
|---|---|
| Simple hover/focus/pressed/class change | CSS transition with explicit properties |
| Declarative repeating or short sequence | CSS keyframes |
| Entry/exit from hidden | Native dialog/popover semantics, enhanced with `@starting-style` and discrete transitions when supported |
| Pause, reverse, cancel, or runtime playback control | Web Animations API |
| DOM swap or navigation continuity | Feature-detected View Transitions, with immediate normal-update fallback |
| Intrinsic-size expansion | Native disclosure or measured CSS solution; consider `interpolate-size` when supported |
| Gestures, shared layout, spring orchestration | Existing project library; consider Motion if a new dependency is justified |
| Scroll-linked decoration | Feature-detected scroll timelines; usable static fallback |
| Required authored or 3D assets | Existing engine or appropriately scoped runtime; lazy-load and provide static fallback |

No library is universally forbidden. Do not migrate GSAP, Framer Motion, or another functioning dependency just to follow this skill. Verify installed-version APIs and browser support when using specialized features; avoid stale browser-version tables.

For authored assets (Lottie, Rive) or 3D scenes, avoid adding them to solve ordinary loading. If the task requires them, load only needed assets, pause offscreen loops, respect reduced motion, provide a poster/static fallback, and include their decode/runtime cost in the performance budget.

## Rendering cost

- Prefer transforms and opacity, particularly for continuous movement. They commonly allow compositing, but actual performance depends on browser, surface area, layers, and effects.
- `filter`, blur, backdrop effects, masks, and large shadows can be expensive. Do not assert that a property is always free or GPU-only.
- `interpolate-size` enables interpolation; it does not guarantee that animating height avoids layout. Grid-track animation can also cause layout. A small measured disclosure transition may still be appropriate.
- FLIP, library layout animation, and View Transitions can use snapshots/transforms to explain a layout change. The initial/final DOM layout still has costs; profile them.
- Avoid `transition: all`, repeated forced layout, and unbounded per-item layers. Batch measurements before writes.
- Use `will-change` only after evidence of need, on a limited set of elements, and remove it after completion or cancellation. Do not add permanent `translateZ(0)` everywhere.
- Keep pointer/scroll values out of per-frame React state where practical; use refs, motion values, or suitable platform APIs. Clean up subscriptions and animation handles on unmount and replacement.
- Do not scale text or pointer targets as a substitute for needed document reflow if it distorts readability or leaves hit areas in the wrong place.

## State choreography and interruption

For shared layout transitions, list reorders, or grid-to-detail changes:

1. Keep stable item identity and determine start/end layout and focus behavior.
2. Commit the logical state promptly. Coordinate optional visual continuity without withholding data or blocking interaction for the whole duration.
3. Retarget, reverse, finish, or cancel previous motion on new intent. Never accumulate an unbounded queue of animations.
4. Keep entry and exit nodes from creating duplicate accessible content, duplicate IDs, or two interactive copies. Exit-only visual snapshots must be inert/noninteractive and excluded from assistive technology.
5. Preserve or deliberately transfer focus to a meaningful target. On closing a dialog, follow the component's established focus-return semantics.
6. Ensure cancellation, navigation, reduced motion, and unsupported APIs still reach the correct final state. Business correctness cannot depend solely on `transitionend` or `animationend`.

Do not hide a focusable element with opacity alone. It may remain in keyboard order and the accessibility tree. Use native hidden/closed state, unmounting, or a carefully scoped `inert`/visibility strategy as appropriate. Coordinate exit visuals separately from semantic removal.

For React, preserve stable keys, avoid duplicating domain state solely to animate, use the existing data layer for async state, and respect client/server boundaries. Strict Mode setup/cleanup must not start duplicate jobs or leak observers. Choose presence modes intentionally: an exit-before-entry mode can add unnecessary latency when the next screen is already ready.

## Reduced motion and access

- Honor `prefers-reduced-motion` in CSS and any JS-driven library. Handle preference changes during the session for active long-running effects.
- Replace decorative translation, zoom, parallax, looping shimmer, and smooth scrolling with static final states or a short optional fade. Preserve status text and meaningful final state.
- Prefer scoped rules over a global `*` reset that unintentionally breaks widgets or animation-dependent code. No motion is required merely to preserve feedback.
- Do not delay focus indicators behind transitions. Support keyboard activation, clear focus, and adequate control labels.
- Preserve non-color feedback. Keep success, failure, and pending states distinguishable by text or appropriate icons.
- Avoid flashing effects. Reduced-motion handling alone does not establish flash safety or WCAG compliance.
- Do not describe every missing motion query as a WCAG AA violation: animation-from-interactions criterion 2.3.3 is AAA; other motion/flash requirements have their own applicability. Implement the relevant protections without claiming a complete accessibility audit.

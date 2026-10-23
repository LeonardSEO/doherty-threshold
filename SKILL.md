---
name: doherty-threshold
description: Design, implement, or review responsive web interactions with immediate input feedback, reliable optimistic updates, intent-based prefetching, skeleton loading, honest progress, and accessible CSS-first motion. Use for sluggish controls, loading states, background saves, or UI animation; preserve the existing stack and design language.
---
# Doherty Threshold

Make the interface acknowledge the user immediately, reveal useful results as soon as they exist, and keep background work understandable. Use motion to explain change without adding waiting.

## The five rules

1. **Every tap gets an immediate response.** Reflect input on the triggering control, ideally in the next paint and within about 100 ms. Support keyboard activation too; a disabled action must explain why it is unavailable.
2. **Show the change now; save in the background when safe.** Optimistically update reversible, low-risk state. Reconcile server truth, preserve user input, and provide recovery on failure. Confirm consequential or server-dependent outcomes before claiming success.
3. **Start slow work before it is needed.** Prefetch likely, read-only next steps on intent; deduplicate and cache. Keep speculative work behind current interaction, network, privacy, and freshness needs.
4. **Show the shape of loading content.** When a new region has no useful content and its layout is known, use a quiet grey skeleton that reserves the final space. Keep useful existing content visible during refresh.
5. **Make long waits legible.** Show real progress when measurable. Otherwise show a clearly indeterminate bar or current phase, without invented percentages; offer cancellation or a background-job path when supported.

These are defaults governed by the actual interaction, not requirements to add all five patterns everywhere.

## Scope and success criteria

Apply within the user's requested component, flow, or review. Inspect existing styles, framework, query/cache tools, and browser targets first. Reuse them; do not migrate libraries or redesign the product merely to follow this skill. Ordinary controls may need no animation at all.

Treat 400 ms as a useful responsiveness heuristic, not a universal scientific cliff, API deadline, or Core Web Vitals threshold. Distinguish:

- **Acknowledgment:** action to first visible feedback; target about 100 ms or less.
- **Useful result:** action to usable updated content; aim below about 400 ms for common local interactions and optimize real latency when longer.
- **Completion:** actual server or background-job outcome; never substitute an animation for confirmation.
- **Motion:** onset, duration, and interruption; a 250 ms transition can begin immediately without postponing work.

## Work from the interaction

For each affected interaction, establish the action, reversibility, initiator, spatial source, and information the feedback must communicate. Infer routine choices from the existing UI; ask only when missing product semantics materially affect correctness.

Trace **input → acknowledgment → pending work → confirmed result or recoverable failure**. Include interruption by another action. Identify the actual bottleneck before selecting a loader or animation.

Choose the simplest useful pattern:

| Situation | Default |
|---|---|
| Local toggle, selection, or open/close | Update state immediately; short optional control transition |
| Reversible preference or inline edit | Optimistic state + background save + reconciliation and recovery |
| Payment, permission change, server validation, irreversible action | Immediate pending feedback; await confirmation for success |
| Cold load of structured content | Render page shell and shape-matched skeleton for the missing region |
| Background refresh or filter with useful previous results | Keep content, distinguish pending/stale state, discard outdated responses |
| Long operation with measurable work | Labelled determinate progress tied to actual completed work |
| Unknown work duration or total | Labelled indeterminate bar or phase status; no made-up completion rate |
| User asks only for a hover/fade or local motion | Use the motion reference; do not introduce unrelated async infrastructure |

### Load only the relevant reference

- Async operations, loader timing, optimistic changes, or prefetch: read [Async feedback](references/async-feedback.md).
- Animation choices, state morphs, browser APIs, or frame performance: read [Motion](references/motion.md).
- Before claiming an implementation works: use the relevant checks in [Verification](references/verification.md). Scale checks to the change.
- Source comparison, design tradeoffs, and attributions: [Sources and decisions](references/sources-and-decisions.md). This is provenance, not required reading for ordinary use.

## Timing and waiting

Use measured timings when available. These bands are starting points, not promises:

| Time since input | Feedback policy |
|---|---|
| Immediately | Pressed/selected/open/pending state, localized to the action |
| Roughly first 150–250 ms | Usually omit an additional loader; cancel its timer if already finished |
| Continuing beyond roughly 250–400 ms | Reveal a local indicator, or a skeleton for missing structured content |
| Roughly 1–3 seconds | Add clear status when needed; retain any useful partial result |
| Beyond several seconds | Progress or phase information, with supported cancel/retry paths |
| Long-running job | Let the user continue elsewhere if the backend supports durable jobs |

A known slow cold load may show its skeleton with the first shell paint. A stalled fast request must still acquire feedback. Never delay useful content, errors, or re-enabled controls merely to satisfy a minimum loader duration. Do not show a full-page skeleton over useful cached content.

## Motion and access essentials

- Prefer CSS transitions/keyframes for simple effects, WAAPI for imperative playback, and existing animation tools for gestures or layout coordination. Add dependencies only for a demonstrated need.
- Prefer `transform` and `opacity` for frequent motion. Filters, intrinsic-size transitions, and grid/height animation are not guaranteed compositor-only; profile instead of promising GPU acceleration.
- Acknowledge before expensive synchronous work. Making a handler `async` does not move CPU work off the main thread.
- Animate toward or from a meaningful source. Keep frequent feedback small, exits brisk, transitions interruptible, and ready controls usable. Do not serialize data fetching behind an exit animation.
- Respect `prefers-reduced-motion` in CSS and JavaScript: use a static final state or short fade, keep information and controls available, and avoid decorative movement or shimmer.
- Preserve focus, keyboard behavior, semantics, and labels. Use text/icon feedback alongside color. Hide decorative skeleton shapes from assistive technology; label busy regions and progress.
- Never depend on an animation-end callback alone for business state, cleanup, or focus recovery; reduced motion, cancellation, or unsupported features can skip it.

## Finish with evidence

For implementation, deliver the scoped change, check relevant success/failure/interruption paths, and report what was measured versus assumed. For review, identify concrete affected interactions, user impact, recommended changes, and verification limits. Do not claim an improved response time from a still screenshot or code inspection alone.

This skill is a rewritten synthesis of the three requested sources; see the source reference and bundled license notices.

# Doherty Threshold

An agent skill for web interfaces that respond immediately, handle background work reliably, and use motion with purpose.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Bring perceived performance and interaction design into the same workflow: acknowledge input, reveal useful results early, and communicate pending work without losing user trust.

## Install

Run from your project directory:

```bash
npx skills add LeonardSEO/doherty-threshold --skill doherty-threshold
```

Or use the full GitHub URL:

```bash
npx skills add https://github.com/LeonardSEO/doherty-threshold --skill doherty-threshold
```

Choose your coding agent when prompted. For a global Codex installation:

```bash
npx skills add LeonardSEO/doherty-threshold --skill doherty-threshold --agent codex --global
```

Requires Node.js 22.20 or later for the current [Skills CLI](https://github.com/vercel-labs/skills). The skill itself consists of Markdown instructions and has no runtime dependencies. Installation support depends on your coding agent; it does not require adding an animation library to your application.

## The five rules

1. **Respond to every tap immediately.** Acknowledge input on the control, ideally in the next paint and within about 100 ms. Include keyboard interaction.
2. **Show the change now; save in the background when safe.** Use optimistic updates for reversible, low-risk actions, with reconciliation and recovery if saving fails.
3. **Start slow work before it is needed.** Prefetch likely read-only next steps on intent, without competing with the current task.
4. **Show the shape of loading content.** Use quiet grey skeletons for missing content with a known layout. Keep useful existing content visible during refresh.
5. **Make long waits understandable.** Show measured progress when possible; otherwise use an explicitly indeterminate bar or phase status.

These are contextual defaults. A simple hover effect does not need an async architecture, and a fast completed request does not need a loader.

## Use

In Codex, invoke the skill explicitly:

```text
Use $doherty-threshold to improve the responsiveness of this checkout flow.
```

Other agents expose installed skills through their own invocation and discovery mechanisms. Example requests:

```text
Review this product filter. Keep previous results visible while the next query
loads, and prevent older responses from replacing newer results.
```

```text
Make this settings toggle update immediately and save in the background.
Preserve the user's latest choice if requests fail or finish out of order.
```

```text
Improve loading feedback for this dashboard. Use skeletons that match the
final layout, avoid unnecessary spinner flashes, and respect reduced motion.
```

```text
Add a subtle opening transition to this dialog. Preserve keyboard focus,
support interruption, and keep the existing animation stack.
```

## What it covers

| Area | Guidance |
|---|---|
| Immediate feedback | Separate acknowledgment, usable results, and confirmed completion |
| Background saves | Pending/saved/error states, scoped rollback, ordered or versioned writes |
| Loading | Delayed indicators, shape-matched skeletons, cached content and partial rendering |
| Early work | Intent-based prefetching, deduplication, cache freshness and bounded speculation |
| Progress | Real work units, distinct processing phases, truthful indeterminate states |
| Motion | CSS-first choices, suitable timing, interruption and rendering cost |
| Accessibility | Reduced motion, focus, keyboard behavior, status and progress semantics |
| Verification | Fast, slow, failed, interrupted and out-of-order interactions |

The skill preserves the existing framework, libraries, and design language. Supporting references are loaded only when relevant.

## How it interprets the threshold

The commonly cited **400 ms** figure is a useful design heuristic, not a universal scientific cutoff or a guarantee that every API call will finish within that time.

This skill distinguishes immediate acknowledgment from usable results and actual completion. It favors reducing real latency and making unavoidable waits understandable. It does not hide ready content to satisfy a minimum loader duration, invent progress percentages, or claim success before a consequential action is confirmed.

Timing suggestions are starting points to validate on representative devices and networks. See the [verification guide](references/verification.md) for the distinction between interaction feedback, async result latency, and browser performance metrics.

## Contents

```text
SKILL.md
agents/openai.yaml
references/
  async-feedback.md
  motion.md
  verification.md
  sources-and-decisions.md
licenses/
```

Start with [SKILL.md](SKILL.md). Read [async feedback](references/async-feedback.md) for loading and persistence, or [motion](references/motion.md) for animation decisions.

## Sources and attribution

This skill is a rewritten synthesis of the following open-source skills:

- [owl-listener/designer-skills — doherty-threshold](https://github.com/owl-listener/designer-skills/tree/9a6930cf84a822eb458624bd11c61aac5bbdf224/interaction-design/skills/doherty-threshold)
- [NVlabs/Skill2Env — doherty-threshold](https://github.com/NVlabs/Skill2Env/tree/507eab4d9952968de2be98f8729d3c66d487285a/SkillHub/skills/doherty-threshold), attributed by its source inventory to Infrasity
- [mthines/agent-skills — animations](https://github.com/mthines/agent-skills/tree/bdbb51856872a16b3177c065d7ee300b7dc92268/skills/design/animations)

The two Doherty sources share essentially the same core. The synthesis resolves conflicting advice and adds explicit handling for concurrency, failure recovery, honest progress, and interruption. It has not been benchmarked as universally superior to its sources.

Pinned revisions, technical references, and the rationale for the changes are recorded in [Sources and decisions](references/sources-and-decisions.md).

## Contributing

Issues and pull requests are welcome. Describe the interaction, expected behavior, device or browser conditions, and an observable example. Changes should keep the core concise, preserve correctness and accessibility, and avoid unsupported performance claims.

## License

[MIT](LICENSE). Upstream copyright and permission notices are preserved in [licenses/](licenses/).

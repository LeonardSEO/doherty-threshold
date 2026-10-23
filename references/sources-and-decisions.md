# Sources and synthesis decisions

Rewritten and substantially modified on 2026-10-23. This is a synthesis of the three requested skill entrypoints and selected supporting animation rules, not an unchanged concatenation or an empirical claim of superiority.

## Pinned source snapshots

| Requested source | Revision | Useful contribution |
|---|---|---|
| [owl-listener/designer-skills](https://github.com/owl-listener/designer-skills/blob/9a6930cf84a822eb458624bd11c61aac5bbdf224/interaction-design/skills/doherty-threshold/SKILL.md) | `9a6930cf84a822eb458624bd11c61aac5bbdf224` | Local acknowledgment, response budgets, real-device measurement |
| [NVlabs/Skill2Env](https://github.com/NVlabs/Skill2Env/blob/507eab4d9952968de2be98f8729d3c66d487285a/SkillHub/skills/doherty-threshold/SKILL.md) | `507eab4d9952968de2be98f8729d3c66d487285a` | Same Doherty core, including fallback feedback |
| [mthines/agent-skills](https://github.com/mthines/agent-skills/blob/bdbb51856872a16b3177c065d7ee300b7dc92268/skills/design/animations/SKILL.md) | `bdbb51856872a16b3177c065d7ee300b7dc92268` | Interaction-to-motion reasoning, CSS-first tools, async patterns, accessibility and measurement |

The two Doherty entrypoints share essentially the same body; they are not independent evidence of the timing claims. The NVlabs SkillHub inventory attributes this particular skill to [Infrasity-Labs/dev-gtm-claude-skills](https://github.com/Infrasity-Labs/dev-gtm-claude-skills), under MIT. The repository-wide NVlabs Apache license is not the inventory's license for this collected skill.

All three entrypoints were read. Supporting animation material consulted for the synthesis includes perceived-performance, interaction-feedback, safe-properties, timing-easing, accessibility, and modern-css. Only the rewritten synthesis and its focused references are distributed here; upstream templates and unrelated reference files are not bundled.

## Deliberate improvements

- Five practical rules form one interaction contract; latency and motion no longer have competing mandatory workflows.
- Replace categorical timing bands with tunable starting points and separate acknowledgment, useful result, completion, and motion duration.
- Avoid an unconditional loader visibility floor: delay an indicator's onset rather than adding waiting after data is ready.
- Keep cached content during refresh instead of replacing it with skeletons. Reveal ready regions without decorative sequencing delays.
- Add operation identity, ordered/versioned writes, scoped rollback, recovery, and truthful saved/pending/error states.
- Keep prefetch read-only, bounded, deduplicated, and appropriate to user intent, current work, privacy, and cache freshness.
- Use determinate progress only for meaningful measurable work. Unknown work gets indeterminate or phase feedback.
- Correct blanket GPU claims: filters and intrinsic-size interpolation may still require expensive rendering work.
- Preserve project libraries and use feature detection rather than prescribing a migration or fixed browser support table.
- Make reduced motion, focus preservation, semantic hiding, and interruption part of the actual interaction contract.
- Remove mandatory dependencies on unavailable companion skills, unrelated redesigns, and automatic recording loops.
- Do not carry over historical claims that 400 ms is an experimentally universal cliff or that speed makes an application addictive.

## Primary technical references

These informed corrections; check current documentation for specialized APIs before implementation.

- [Google: INP](https://web.dev/articles/inp) — next-paint responsiveness and the distinction from later async results.
- [Google: RAIL](https://web.dev/articles/rail) — context-specific responsiveness and frame budgets; the page recommends Core Web Vitals for current performance goals.
- [Google: performant animations](https://web.dev/articles/animations-guide) — rendering cost, transform/opacity, profiling and cautious use of will-change.
- [MDN: interpolate-size](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/interpolate-size) — intrinsic-size interpolation and browser support.
- [MDN: progressbar](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/progressbar_role) — labelled progress and determinate/indeterminate semantics.
- [W3C: animation from interactions](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html) — reduced motion and the AAA criterion's scope.

## License notices

Preserved MIT notices: [owl-listener](../licenses/owl-listener-MIT.txt), [mthines](../licenses/mthines-MIT.txt), and [Infrasity](../licenses/Infrasity-MIT.txt). These preserve upstream copyright and permission terms for adapted material. No assets or source code from the source animation templates are incorporated in this skill.

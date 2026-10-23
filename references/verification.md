# Verification

Select checks that match the affected behavior. Do not require a full performance lab for a three-line hover change, but do not claim responsiveness or async correctness without relevant evidence.

## Observable behavior

For a changed async flow, exercise:

- Fast completion: control acknowledges, result appears without an unnecessary loader or artificial hold.
- Normal and delayed completion: correct local placeholder/status; useful existing regions remain available.
- Failure/offline: correct pending state ends, input remains recoverable, retry is clear, and no success is falsely reported.
- Out-of-order reads and rapid repeated writes: an older result or rollback does not overwrite newer intent; server persistence matches the intended final state.
- Cancellation/navigation/unmount: no late loader, orphan timers, stale result, duplicate job, or misleading claim that server work stopped.
- Known/unknown progress: percentage matches actual work; unknown totals are labelled indeterminate; upload and processing phases remain distinct.

For changed motion, check normal and reduced motion, keyboard/focus behavior, immediate interruption/reversal, and a usable fallback for any newly used unsupported feature. A local reduced-motion emulation is useful evidence; it is not a complete assistive-technology audit.

## Measurement

Measure from input to visible feedback and separately to usable result/confirmed completion. A fast server response does not prove a fast next paint. Capture a browser performance trace if there is meaningful main-thread or frame risk.

- Use field data when available, with p75 and relevant slow tails segmented by representative device/network. If unavailable, simulate realistic conditions and label the results as lab observations.
- Track INP for responsiveness and custom interaction marks for async result latency. INP does not include every later network-dependent result. The web.dev good-INP threshold is at most 200 ms at the 75th percentile, distinct from the approximately 100 ms acknowledgment goal and the 400 ms heuristic.
- Inspect layout shift at placeholder swaps and image/font delivery. Confirm that geometry is reserved at relevant breakpoints.
- For continuous animation, a display frame is approximately 16.7 ms at 60 Hz or 8.3 ms at 120 Hz, shared by all rendering work. Budget app work below that total; do not call the whole frame a JavaScript allowance.
- Inspect long tasks, repeated style/layout work, large paint regions, layer memory, and dropped frames. An initial/final layout is normal; sustained repeated work is the diagnostic concern.

A still image can show shape, colors, and geometry. Use an interaction trace or recording when the claim depends on timing, interruption, or motion. Use available tools; do not assume separate recording/analyzer skills are installed.

## Concise delivery

Report the interaction changed, its new feedback and failure behavior, checks performed, any measured values and conditions, and material unverified limitations. Do not claim a performance improvement, 60 fps, accessibility compliance, or production validation that was not measured.

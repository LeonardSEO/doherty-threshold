# Async feedback

Read for loading states, background saves, speculation, and progress. Prefer existing framework primitives rather than adding a new state or query library.

## Acknowledgment and actual latency

Respond on the control first, then start the work without blocking the next paint. Moving a fetch earlier, reducing waterfalls, caching, streaming independent regions, deferring nonessential code, and optimizing expensive rendering can improve real latency. A loader only explains the remaining wait.

For CPU work, profile the handler; split or yield work appropriately, or use a worker for suitable computation. A promise wrapper or `async` keyword alone does not yield heavy computation to rendering.

Use a compact state model such as `idle → pending → success | error`, with operation identity where concurrent requests are possible. Derive visible state from the current operation rather than unrelated booleans. Distinguish empty, loading, error, stale, and loaded states.

## Loader timing without added waiting

Use a delayed indicator for a normally fast operation:

1. Acknowledge immediately and start the actual request.
2. Schedule the additional local indicator for approximately 150–250 ms later; tune to observed waits and the component.
3. If still current and pending at that time, show it. Escalate the status if the operation continues much longer.
4. On result, show useful content or the error immediately; clear timers and subscriptions on settlement, replacement, or unmount.
5. Ignore callbacks from replaced operations. A late timer must not resurrect loading after completion.

An immediate skeleton is appropriate for a known slow cold region rendered with the page shell. It does not require delaying all shell content behind an arbitrary timer.

Avoid flashes by delaying the loader's onset, reserving geometry, or gently fading its decorative layer. Do not enforce an unconditional 200 ms minimum visible loader period that withholds already available content. Any decorative exit must not intercept input, obscure useful results, or delay error recovery.

## Skeletons and partial rendering

- Match the actual responsive layout: card geometry, table rows, avatar/text structure, and image aspect ratios. Adapt placeholders to narrow screens too.
- Reserve dimensions and use low-contrast neutral grey appropriate to the theme. Keep labels and navigation real where available.
- Static placeholders are sufficient. If shimmer helps during a longer wait, move a subtle overlay using transform and disable it for reduced motion.
- Hide decorative shapes with `aria-hidden="true"`; put `aria-busy` on the region being updated and provide one useful loading status. Clear busy state on both success and failure.
- Reveal independently useful regions as they arrive. Do not wait for every request to finish or stagger ready text just for visual effect.
- Keep previous usable results during a refresh. Indicate that an update is pending when old data could otherwise be mistaken for the new query's result.
- If content shape is unpredictable, use a labelled local status instead of implying a precise result count or misleading layout.
- Ensure ready content is interactive and accessible, even if a decorative fade is still ending.

## Optimistic updates: correctness before polish

Good candidates include reversible preferences, likes, reordering, and editable local data with a clear reconciliation strategy. High success probability helps, but there is no universal success-rate cutoff. Collaborative state can be optimistic only when the existing synchronization protocol supports it.

For consequential, irreversible, or server-authoritative outcomes, show pending feedback immediately and wait before claiming completion. Examples include payment confirmation, permission grants, username availability, and validated pricing. Local drafts can still update immediately.

For a candidate mutation:

1. Preserve the user's desired value and affected prior state or patch. Apply the local change now.
2. Send an explicit desired state where supported (`liked: true`), rather than a retry-sensitive toggle command. Deduplicate repeated submissions; use existing idempotency support for consequential retries.
3. Mark the operation pending when persistence matters. Distinguish `Saving…`, `Saved`, and `Not saved` honestly.
4. On success, reconcile the canonical server state and invalidate only relevant cached data.
5. On failure, roll back only the affected optimistic change or retain an explicitly unsaved draft with retry. Explain the failure; preserve entered text and offer a useful recovery path.
6. Protect newer changes from older responses. Serialize conflicting writes, use version checks, or use the data layer's supported reconciliation strategy. Do not restore an old whole-list snapshot over subsequent edits.

Do not claim a query library automatically solves every race. Check its documented mutation/invalidation behavior for the installed version. Cancel obsolete reads where possible; cancellation on the client does not guarantee that a server mutation was undone.

For autosave, debounce persistence when helpful, not the visible editing response. Preserve unsaved state across failures; choose serialization/versioning for writes so out-of-order completion cannot persist an older value. Warn about leaving only when there is actual unsaved work that could be lost, consistent with the app's conventions.

## Start slow work early

Prefer existing router/query prefetch features; inspect their current behavior rather than assuming every framework prefetches the same way.

- Trigger likely read-only requests on intentional hover, keyboard focus, pointer-down, or a predictable next step. Touch flows need a useful path without hover.
- Debounce incidental pointer passes; cancel a scheduled speculative fetch when intent disappears. Deduplicate in-flight work and reuse valid cached data.
- Prioritize the current action, above-the-fold content, and primary image. Limit concurrency and speculative byte cost. Respect available Save-Data and connection hints; their absence does not prove unlimited bandwidth.
- Never speculate mutations, destructive actions, consent decisions, or other side effects. HTTP GET alone does not prove a particular endpoint is safe.
- Key caches by user/account, parameters, and permissions. Invalidate appropriately after mutations and account changes; do not expose another user's cached data.
- Revalidate stale data only when the domain permits it. Availability, prices, and authorization may need explicit fresh checks before committing an outcome.

For progressive delivery: stream independent regions; reserve image geometry; decode asynchronously where appropriate; lazy-load below-fold assets; choose suitable fallback-font metrics. Do not lazy-load the primary above-fold image simply because lazy loading is available.

## Progress that tells the truth

Use a determinate bar only when a meaningful total and completed work are known. Bind it to bytes, completed items, or accurately defined stages; explain the unit. Do not interpolate a fake percentage toward 90% and wait there.

- Upload completion is not processing completion. If the server continues working after transfer, change the label and progress mode to reflect that phase.
- For unknown totals, show an indeterminate bar with useful phase text; omit `aria-valuenow` for a custom indeterminate progressbar. A native `<progress>` without `value` is indeterminate.
- For measurable work, prefer a labelled native `<progress value="4" max="12">` or supply an accessible name and valid range/value for a custom progressbar. Keep a readable status outside the progressbar itself.
- Under reduced motion, an indeterminate bar may be static with a clear processing label. Reduced animation must not imply the work has stopped.
- Announce significant milestones through a polite status region; avoid a live-region update for every percentage point. Completion and failure need clear text.
- Use an ETA only when there is a defensible estimate; label it approximate. Never mark 100% as successful before the actual operation is confirmed.
- Expose cancellation when the system can honor it. If only waiting can be cancelled, say so; do not imply the server job was cancelled. Enable retry only when its effects are understood.
- For durable long jobs, provide a job/status link and allow navigation. Promise a notification only if the delivery mechanism exists and is authorized.

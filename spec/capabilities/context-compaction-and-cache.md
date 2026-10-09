# Capability — Context, Compaction, And The Prompt Cache

> **CAPABILITY SPECIFICATION.** Behavior and invariants, free of implementation detail. This document
> defines how the model's view of a transcript is derived from the durable log, how a transcript is
> compacted in the background and frozen into the blob store, and how the platform manages the prompt
> cache, the largest cost. Requirements realize [Platform Principle P2](../../constitution.md) and
> [Platform Principle P7](../../constitution.md) and trace to [overview §6](../overview.md) and
> [overview §5](../overview.md).
>
> RFC-2119 key words are normative. Each requirement is a single self-contained sentence carrying exactly
> one obligation, under a stable heading. Thresholds, lifetimes, and the compression scheme are declared
> defaults.

## Purpose And Scope

A transcript grows without end, a prompt cache expires in idle gaps, and both cost tokens. This capability
fixes the derived views (rollback, trimming, bounded results), background compaction, freezing, and the
cache policy, all configured per session from its definition.

## The Model's View Is Derived

### The Log Is Never Edited

The model's view of a transcript MUST be derived from the durable log.

The durable log MUST NOT be edited in place.

### Rollback Removes Attempts From The View Only

A rollback MUST remove failed attempts from the model's view only.

The durable log MUST keep rolled-back attempts for audit and observation.

Where a provider binds reasoning to the history it was produced in, a rollback MUST drop the bound
reasoning rather than send it with an edited history.

### Trimming Leaves A Resolvable Placeholder

A stale tool result trimmed from the view MUST be replaced by a placeholder the model can resolve by the
call id.

Trimming MUST evict the oldest stale results first.

### Tool Results Are Bounded In The View

A tool result MUST enter the view as a bounded view whose bounds come from the tool spec.

The full output of a tool call MUST be stored once and paged through by a native tool.

## Compaction

### Thresholds Are Per Session And Well Below The Window

A session's definition MUST set a soft threshold at which a background compaction starts.

A session's definition MUST set a hard threshold at which compaction becomes a precondition of the next
turn.

Both thresholds MUST be below the model's context window.

A session's soft compaction threshold MUST be lower than its hard compaction threshold.

The soft threshold starts compaction before the hard admission hold is needed. Together with rising
compaction priority near the hard threshold and pre-idle compaction, this is intended to make such
holds rare; it is not a numeric margin, latency target or guarantee that every hold is avoided.

### Compaction Runs In The Background

A compaction MUST be a side request that covers the transcript up to a cut point.

A compaction result MUST be swapped in only at a turn boundary.

A session MUST keep working while its compaction runs only while the hard compaction threshold does
not require the next turn to wait.

Cancelling a compaction MUST NOT change the session's durable state.

Cancelling a compaction MUST take effect by the next turn boundary.

An event that clears the session's interrupt threshold MUST preempt a running compaction, which is then
rescheduled.

A compaction's priority MUST rise as the context nears the hard threshold, so that a stream of events
cannot starve it.

A turn that arrives during a compaction MUST run on the uncompacted context only when the hard
compaction threshold does not require it to wait.

A turn held by the hard compaction threshold MUST wait until a successful compaction has made its
admission permissible.

### The Swap Preserves History

Swapping in a compaction MUST replace everything before the cut with the summary and keep the tail
verbatim.

The history before the cut MUST be frozen into the blob store rather than deleted.

The swap MUST apply pending changes to the cached prefix, such as tool and system prompt changes, in the
same rewrite.

### The Keep Set Stays Verbatim

A compaction MUST keep the recent tail verbatim.

A compaction MUST keep the keep set verbatim: plans, open items, unresolved defects, constraints,
decisions, and identifiers the work depends on.

### Method

A compaction MUST use the provider's native compaction where the provider offers one.

The platform's own summarizer MUST be used only as a fallback.

A compaction MUST count against the session's budget.

A compaction MAY run in the batch latency class.

### A Charter Change Schedules A Compaction

A change to a session's charter MUST schedule a low-priority compaction that folds the change into the
system prompt.

Charter changes MUST be debounced, so that a burst of them costs one compaction.

A compaction that follows a charter change MUST write its summary under the new charter.

A charter change arriving while a compaction is already due MUST ride that compaction.

### Compaction Failures Are Bounded

A compaction that fails MUST be retried within the configured attempts.

A compaction a turn depended on that exhausts its attempts MUST block the session with the reason.

## Freezing

### Stable Ranges Move To The Blob Store

A transcript range that stabilizes, by passing a compaction boundary or a size threshold, MUST be frozen
into the blob store.

A frozen range MUST be encoded, compressed, and encrypted before it is written.

The durable log MUST keep only a pointer to a frozen range: the blob name, the range, and its sizes.

An idle session MUST be able to freeze entirely, leaving a pointer record.

Freezing triggers MUST be per-session configuration.

### Frozen History Is Recoverable

A session's history MUST be recoverable from the blob store and the key service alone.

The transcript ranges observation records name MUST be the same frozen blobs.

## The Transcript Cache

### Every Tier Above The Log Is A Cache

A rendered segment held in memory or on local storage MUST be a cache keyed by content.

Dropping any tier of the transcript cache MUST lose nothing.

## The Prompt Cache Policy

### The Policy Comes From The Definition

A session's cache policy MUST come from its definition.

### The Platform Manages The Lifetime

The platform MUST choose a session's cache lifetime per session, taking a longer lifetime where the front
door offers one and the session's wake history says it pays.

The platform MUST keep a prefix warm across an expected short gap with a refresh read before it expires,
when the next wake is likely inside the window.

The platform MUST let a prefix expire when the next wake is not expected inside the window.

### The Platform Avoids Cold Rewrites

The platform MUST compact before a long idle, so that the rewrite on the next wake is small.

A low-priority wake MAY be held for a warm window or folded into the session's next wake, where its
deadline allows.

### Every Lever Is Measured

Every usage record MUST carry the cache reads and writes of its attempt, so that the policy is measured
per session and per definition.

Every cache lever MUST be configuration, so that a change to it rolls out as a definition version.

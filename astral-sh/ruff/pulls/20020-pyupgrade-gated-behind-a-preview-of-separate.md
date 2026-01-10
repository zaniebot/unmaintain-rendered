```yaml
number: 20020
title: "[`pyupgrade`] Gated behind a preview of separate diagnostics (`UP010`)"
type: pull_request
state: closed
author: IDrokin117
labels:
  - preview
assignees: []
base: main
head: feat/ruff-19561-preview
created_at: 2025-08-21T10:52:05Z
updated_at: 2025-10-06T06:54:33Z
url: https://github.com/astral-sh/ruff/pull/20020
synced_at: 2026-01-10T17:34:34Z
```

# [`pyupgrade`] Gated behind a preview of separate diagnostics (`UP010`)

---

_Pull request opened by @IDrokin117 on 2025-08-21 10:52_

## Summary

Second part of https://github.com/astral-sh/ruff/pull/19769

Separate diagnostic generation for each unused import and range updating accordingly. Gated behind a preview

## Test Plan

Added preview snapshots


---

_Comment by @github-actions[bot] on 2025-08-21 11:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2025-08-21 11:02_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/IDrokin117%3Afeat%2Fruff-19561-preview)

### Merging #20020 will **not alter performance**

<sub>Comparing <code>IDrokin117:feat/ruff-19561-preview</code> (affcf04) with <code>main</code> (542f080)</sub>



### Summary

`✅ 30` untouched  
`⏩ 13` skipped[^skipped]  



[^skipped]: 13 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/IDrokin117%3Afeat%2Fruff-19561-preview?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Converted to draft by @IDrokin117 on 2025-08-21 12:28_

---

_Marked ready for review by @IDrokin117 on 2025-08-21 13:05_

---

_Comment by @IDrokin117 on 2025-09-08 18:00_

@ntBre Could you please review PR?

---

_Review requested from @ntBre by @ntBre on 2025-09-08 18:01_

---

_Label `preview` added by @ntBre on 2025-09-08 18:01_

---

_Comment by @ntBre on 2025-09-08 18:01_

Yes, sorry for the delay! We've been preparing for the 0.13 release this past week. I'll be catching up on reviews once that goes out.

---

_Comment by @MichaReiser on 2025-09-19 09:38_

What's the motivation for splitting the diagnostics? This seems unnecessarily verbose to me. Could we instead use the new sub-diagnostics to highlight the individual ranges?

---

_Comment by @ntBre on 2025-09-19 12:53_

What I liked about the split diagnostics in https://github.com/astral-sh/ruff/pull/19769#pullrequestreview-3111691918 was mostly just underlining the affected imports instead of the whole import statement, which I think could also be handled by sub-diagnostics.

As one supporting example, we also emit one diagnostic per import for [unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401). Although we could revisit that as well now that we have sub-diagnostics.

I don't feel too strongly either way, though.

[playground](https://play.ruff.rs/bf87eac4-7c5d-45f5-8183-6b1830a84ed1)

---

_Comment by @IDrokin117 on 2025-09-23 17:38_

@ntBre What should I do here? It seems diagnostics work per import as for F401, doesn't it? Should I use sub-diagnostics instead? What piece of code can I take as a "sub-diagnostic" example?

---

_Comment by @ntBre on 2025-09-25 14:50_

I think we just need to reach agreement on how many diagnostics to emit. If you want to try sub-diagnostics to see how they look, here is a previous comment I used on another PR:

The easiest way is  with `secondary_annotation` on the guard returned by `report_diagnostic`:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/checkers/ast/mod.rs#L3309-L3311

Here's the one use of it we've added so far:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/rules/pyflakes/rules/redefined_while_unused.rs#L195-L198

And an example of the type of diagnostic it produces (the `-----` underlining is used for secondary annotations):

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F811_F811_0.py.snap#L4-L17

There are some other options if that doesn't look good, but that's the easiest place to start.

<hr>

That's not technically a sub-diagnostic, but we've only added a nice helper for secondary annotations rather than `SubDiagnostic`s proper. The helper code would be quite similar to the annotation version, but I'm happy to help with that if we go that route.

---

_Closed by @IDrokin117 on 2025-09-30 19:33_

---

_Reopened by @IDrokin117 on 2025-09-30 19:33_

---

_Comment by @IDrokin117 on 2025-09-30 19:35_

@ntBre @MichaReiser I've rewritten the diagnostic using secondary_annotation. Please check it and let me know how it looks now.  
I've also handled the edge case when there is only one alias in the import statement, so there is no need to underline that specific alias separately.

---

_@ntBre reviewed on 2025-09-30 19:43_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-09-30 19:43_

I think if we do go this route, we should narrow the primary diagnostic range to just `__future__` (or maybe `from __future__` or `from __future__ import`) to keep the two ranges from overlapping:


```suggestion
1 | from __future__ import nested_scopes, generators
  |      ^^^^^^^^^^        -------------  ----------
```

I think I'd still prefer either one diagnostic per import, or just leaving the diagnostic as it is on stable, over this, though. But I'm curious to hear what Micha thinks. Thank you for exploring this!

---

_@MichaReiser reviewed on 2025-10-01 06:28_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-10-01 06:28_

I'd have to double-check but narrowing the range changes the places where suppression comments are allowed. 

E.g, the following currently works but I suspect won't work if we narrow the range

```
from __future__ import (
	nested_scopes, # noqa <RULE>
 	generators
)
``` 

---

_@MichaReiser reviewed on 2025-10-01 06:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-10-01 06:35_

Hmm yeah, using multiple annotated ranges and highlighting the entire import statement doesn't help improve readability.

I only found this usage in cargo:

```
warning: methods `unused1` and `unused2` are never used
  --> src/function/maybe_changed_after.rs:41:19
   |
32 | impl VerifyResult {
   | ----------------- methods in this implementation
...
41 |     pub(crate) fn unused1(&self) {}
   |                   ^^^^^^^
42 |
43 |     pub(crate) fn unused2(&self) {}
   |                   ^^^^^^^
   |
   = note: `#[warn(dead_code)]` on by default
```

They use the `unused1` as the primary range and mark the `impl` block as a secondary annotation. But doing the same in Ruff requires figuring out how to make our suppression system understand that this diagnostic represents two separate violations (the current assumption is that diagnostics only represent a single error.

---

_@ntBre reviewed on 2025-10-01 13:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-10-01 13:19_

Ah, would narrowing the noqa range actually be a benefit of separate diagnostics here? Something like this doesn't currently [work](https://play.ruff.rs/1fa9e588-8306-4480-80d9-f720350e0506):

```py
from __future__ import ( 
    nested_scopes,  # noqa: UP010
    generators,
) 

generators
```

You have to put the noqa [here](https://play.ruff.rs/1d712d9e-8afa-4082-8872-c70379080045):

```py
from __future__ import ( # noqa: UP010
    nested_scopes,  
    generators,
) 

generators
```

at least that's the only place I found that works.

But yeah, I think secondary annotations are probably better for highlighting additional context that helps to explain the main diagnostic but likely falls outside the normal snippet context window, like in your Rust example.

---

_@IDrokin117 reviewed on 2025-10-03 13:52_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-10-03 13:52_

It appears that the previous implementation, which uses separate diagnostics, functions well for `# noqa: UP010` per import object. Should I revert to that approach?

---

_@MichaReiser reviewed on 2025-10-04 12:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-10-04 12:00_

I'm leaning towards leaving it as is. I don't think that any of the tried approaches greatly improve the rendered diagnostics to justify a breaking change. But maybe Brent thinks differently? I'd otherwise close this PR and suggest that we revisit the rendering once we have figured out how to better handle those cases with our diagnostic system. But thank you a lot for trying out all those different approaches. It was very helpful to drive the decision-making and show where there's some need to improve our diagnostic system.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010_0.py__preview.snap`:8 on 2025-10-04 15:21_

Yeah, I think I agree with Micha. Thank you for working on this, I also agree that it was very helpful to see the different options!

---

_@ntBre reviewed on 2025-10-04 15:21_

---

_Closed by @MichaReiser on 2025-10-06 06:54_

---

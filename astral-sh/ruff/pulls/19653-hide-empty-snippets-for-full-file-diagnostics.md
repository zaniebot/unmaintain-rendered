```yaml
number: 19653
title: Hide empty snippets for full-file diagnostics
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/full-file-diagnostics
created_at: 2025-07-30T23:24:47Z
updated_at: 2025-08-05T15:20:33Z
url: https://github.com/astral-sh/ruff/pull/19653
synced_at: 2026-01-10T17:52:17Z
```

# Hide empty snippets for full-file diagnostics

---

_Pull request opened by @ntBre on 2025-07-30 23:24_

Summary
--

This is the other commit I wanted to spin off from #19415, currently stacked on #19644.

This PR suppresses blank snippets for empty ranges at the very beginning of a file, and for empty ranges in non-existent files. Ruff includes empty ranges for IO errors, for example.

https://github.com/astral-sh/ruff/blob/f4e93b63351bfe035b1fc5a7bc605a52979e7841/crates/ruff_linter/src/message/text.rs#L100-L110

The diagnostics now look like this (new snapshot test):

```
error[test-diagnostic]: main diagnostic message
--> example.py:1:1                             
```

Instead of [^*]

```
error[test-diagnostic]: main diagnostic message
--> example.py:1:1
 |
 |
```

Test Plan
--

A new `ruff_db` test showing the expected output format

[^*]: This doesn't correspond precisely to the example in the PR because of some details of the diagnostic builder helper methods in `ruff_db`, but you can see another example in the current version of the summary in #19415.


---

_Label `internal` added by @ntBre on 2025-07-30 23:24_

---

_Label `diagnostics` added by @ntBre on 2025-07-30 23:24_

---

_Comment by @codspeed-hq[bot] on 2025-07-30 23:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Ffull-file-diagnostics?runnerMode=Instrumentation)

### Merging #19653 will **not alter performance**

<sub>Comparing <code>brent/full-file-diagnostics</code> (2cfad84) with <code>main</code> (2db4e5d)</sub>



### Summary

`✅ 42` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-07-30 23:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[WIP] Hide empty snippets for full-file diagnostics" to "Hide empty snippets for full-file diagnostics" by @ntBre on 2025-07-31 20:34_

---

_Marked ready for review by @ntBre on 2025-07-31 20:46_

---

_Review requested from @carljm by @ntBre on 2025-07-31 20:46_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-31 20:46_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-31 20:46_

---

_Review requested from @sharkdp by @ntBre on 2025-07-31 20:46_

---

_Review requested from @dcreager by @ntBre on 2025-07-31 20:46_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-31 20:46_

---

_Review request for @dcreager removed by @ntBre on 2025-07-31 20:46_

---

_Review request for @carljm removed by @ntBre on 2025-07-31 20:46_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-31 20:46_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-31 20:46_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1194 on 2025-08-01 08:19_

Nit: Would it make sense to add an assertion that the snippet has no body (e.g no associated message?)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:718 on 2025-08-01 08:20_

We should mention that this also omits the annotation's message. Or we could phrase it differently and say that everything other than the file name and range are omitted.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:833 on 2025-08-01 08:23_

Can we document the meaning of a file level annotation here too

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:52 on 2025-08-01 08:24_

Do we have any file level syntax errors? I don't think there should be any

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:81 on 2025-08-01 08:28_

Would it be much more work to instead change the rules that add file level diagnostics rather than doing this "global" override? 

(the rule would have to mutate the `Diagnostic` after reporting it with `report_diagnostic`).

The reason I'd prefer this is that it makes it more apparent where we rely on this behavior and it doesn't prevent other rules to use an empty range (e.g. what if we had a rule that enforces module level docstrings. That rule would probably ask you to insert the docstring at 0:0. 

---

_@MichaReiser reviewed on 2025-08-01 08:28_

Thank you. The approach looks good to me but I'm unsure about the ruff integration

---

_@ntBre reviewed on 2025-08-01 13:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:81 on 2025-08-01 13:26_

Ah that's a great idea! We actually do have rules like you're describing. I think at least one import rule suggests adding an import at the top of the file (probably a `__future__` import rule). It might be easier to handle that in #19415 as I'm going through the snapshots, but I can revert this for now.

You're right about the syntax errors too, I don't think we have any file-level ones. That was just for consistency with the current version.

On second thought, we might want to hold off on this until we can cache the `Annotation`s. I don't think `set_file_level` will be cached currently.

---

_@MichaReiser reviewed on 2025-08-01 13:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:81 on 2025-08-01 13:35_

> On second thought, we might want to hold off on this until we can cache the Annotations. I don't think set_file_level will be cached currently.

I'm not sure I understand what you mean by caching.

---

_@ntBre reviewed on 2025-08-01 13:55_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1194 on 2025-08-01 13:55_

Sure, some kind of assertion makes sense to me. I think we can assert that the snippet source is empty, I'll just have to rework the test to avoid using the builder, which expects a non-empty snippet. It looks like Ruff's real diagnostics that will go through this path should already have empty snippets. We could also check that the annotation labels are empty (the text after `^^^`), but I think those are always empty for us right now.

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1194 on 2025-08-01 14:08_

I think `RenderableSnippet::new` might try to expand the snippet to the usual number of context lines, unless we also check `is_file_level` there. We might have to remove or update the assertion when running this in #19415, but I'll add it for now. It'll be a good reminder to revisit it.

---

_@ntBre reviewed on 2025-08-01 14:08_

---

_@ntBre reviewed on 2025-08-01 14:10_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1194 on 2025-08-01 14:10_

Oh, maybe that's in line with your suggestion about applying this per-rule. I guess we shouldn't render one of these except in cases where the snippet isn't available, like an `io-error`!

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:833 on 2025-08-01 14:19_

Good idea, I added a note here about `Span` too, referring to this section of the `Span` docs:
https://github.com/astral-sh/ruff/blob/e7e7b7bf216caf6ea2b323c7484550d736ef1ddb/crates/ruff_db/src/diagnostic/mod.rs#L1080-L1084

---

_@ntBre reviewed on 2025-08-01 14:19_

---

_@ntBre reviewed on 2025-08-01 14:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/mod.rs`:81 on 2025-08-01 14:57_

(also discussed on Discord, just recording here for future reference)

For caching, I just mean in ruff's diagnostic cache. I think we would lose the `is_file_level` field on the annotation when converting to a [`CacheMessage`](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/cache.rs#L454)

I'll drop the syntax error check because that shouldn't affect any real diagnostics and open an issue to follow up on this after a caching PR.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:81 on 2025-08-01 15:30_

Let's maybe add a TODO here too

---

_@MichaReiser approved on 2025-08-01 15:30_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:833 on 2025-08-04 15:26_

Should this get a TODO that it ought to be removed in favor of Ruff diagnostics being updated to use a `None` span?

---

_@BurntSushi approved on 2025-08-04 15:26_

---

_@ntBre reviewed on 2025-08-04 18:42_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:833 on 2025-08-04 18:42_

Ah that's a good point. I added an explicit TODO in the code here and also wrote some notes from looking into the options today: https://github.com/astral-sh/ruff/issues/19688#issuecomment-3151925617. In short, I thought we would handle this with a `None` range on the span, but I think you're right that we should omit the span entirely instead.

It might be a bit tangential to the main issue in #19688, but at least it's recorded somewhere, and I can spin it off later if needed.

---

_@BurntSushi reviewed on 2025-08-05 12:17_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:833 on 2025-08-05 12:17_

Yeah this at least gives us a possible path toward eventually merging back with upstream.

---

_Merged by @ntBre on 2025-08-05 15:20_

---

_Closed by @ntBre on 2025-08-05 15:20_

---

_Branch deleted on 2025-08-05 15:20_

---

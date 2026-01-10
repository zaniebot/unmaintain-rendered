```yaml
number: 17587
title: "Split `SourceLocation` into `LineColumn` and `SourceLocation`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/line-character-column
created_at: 2025-04-23T17:24:50Z
updated_at: 2025-04-27T10:30:55Z
url: https://github.com/astral-sh/ruff/pull/17587
synced_at: 2026-01-10T19:33:02Z
```

# Split `SourceLocation` into `LineColumn` and `SourceLocation`

---

_Pull request opened by @MichaReiser on 2025-04-23 17:24_

## Summary

This PR splits `SourceLocation` into `LineColumn` and `SourceLocation` and moves the `TextSize` to LSP position conversion logic into `LineIndex`.

`LineIndex` as it is before this PR had two methods:

* `source_location`: Converts a `TextSize` to a `SourceLocation`
* `offset: `Converts a `SourceLocation` back to a `TextSize`


The problem with the current implementation is that `source_location` trims a leading BOM offset, whereas `offset` doesn't have any custom BOM handling. 
That means, mapping the first character right after a BOM to the old `source_location` would give `row: 0`, `column: 0`, but mapping that position back to an offset would point **before** instead of **after** the BOM. 

This PR fixes this inconsistency by removing the `offset` for the old `SourceLocation` (now `LineColumn)` because the only case where we need to map back a column is in the formatter but the special BOM handling doesn't matter.

However, we don't want to skip the BOM for LSP operations because LSP operations don't return line/column information; instead, they map a position to a line and the `nth` character on that line. 
This is why this PR introduces a new pair of `source_location` and `offset` methods to map between `TextSize` and a `line` and `character_offset` where `character_offset` is an `UTF8`, `UTF16` or `UTF32` offset (bytes, code units, Unicode scalar values). 

The reason I dove into all this is because the playground needs to convert the ranges to UTF16 and I wanted to avoid copying the whole conversion logic a third time (ruff server, red knot server, wasm)

## Test Plan

* Tested the Ruff and VS code extension with unicode content
* Tested that the line numbers in the CLI are correct
* Tested notebooks
* `cargo test`


---

_Label `internal` added by @MichaReiser on 2025-04-23 17:25_

---

_@MichaReiser reviewed on 2025-04-23 17:27_

---

_Review comment by @MichaReiser on `crates/red_knot_wasm/src/lib.rs`:411 on 2025-04-23 17:27_

I'll update the playground to use `LineCharacter` in a seprate PR

---

_Comment by @github-actions[bot] on 2025-04-23 17:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-23 17:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Split `SourceLocation` into `LineColumn` and `LineCharacter`" to "Split `SourceLocation` into `LineColumn` and `SourceLocation`" by @MichaReiser on 2025-04-23 17:58_

---

_Marked ready for review by @MichaReiser on 2025-04-24 07:31_

---

_Review requested from @carljm by @MichaReiser on 2025-04-24 07:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-24 07:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-24 07:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-24 07:31_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-24 07:31_

---

_Review request for @dcreager removed by @MichaReiser on 2025-04-24 07:31_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-24 07:31_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-24 07:31_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-04-24 07:31_

---

_Review comment by @dhruvmanila on `crates/ruff_source_file/src/line_index.rs`:109 on 2025-04-25 17:41_

Should we be explicit here that this includes the BOM character? i.e., it'll panic if the offset is inside the BOM charcater.

---

_Review comment by @dhruvmanila on `crates/ruff_source_file/src/line_index.rs`:131 on 2025-04-25 17:43_

Does this `LineCharacter` correspond to any data structure as I couldn't find any? We should probably provide a reference for what that is or just use "line character" instead?

---

_Review comment by @dhruvmanila on `crates/ruff_source_file/src/line_index.rs`:320 on 2025-04-25 17:47_

From what I gather the sub-headers in the "Examples" header is related to the content and not the position encoding, it might be worth updating the header to be explicit about it by using something like "ASCII source" or "ASCII text" and similarly down below to "UTF8 source" or "UTF8 text".

---

_Review comment by @dhruvmanila on `crates/ruff_wasm/src/lib.rs`:324 on 2025-04-25 17:52_

Is this now required or can we use `LineColumn` wherever this is being used? Looking at the `impl From`, it seems to be a 1-1 mapping. Or, are you thinking to change this in a follow-up PR for the playground changes? This does require changing the TypeScript types and the frontend code to use "line" instead of "row".

---

_@dhruvmanila approved on 2025-04-25 18:04_

This is great, much better API, thanks for taking a look at this!

I've a few comments but otherwise this looks like a pretty straightforward and logical change to me.

---

_Comment by @dhruvmanila on 2025-04-25 18:04_

Also, apologies for the late review, falling a bit behind on notifications.

---

_@MichaReiser reviewed on 2025-04-26 11:52_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/src/lib.rs`:324 on 2025-04-26 11:52_

I introduced it because I didn't wanted to break the WASM api because there are some projects that use it outside the playground.

---

_@dhruvmanila reviewed on 2025-04-26 15:36_

---

_Review comment by @dhruvmanila on `crates/ruff_wasm/src/lib.rs`:324 on 2025-04-26 15:36_

Understood, that makes sense.

---

_Comment by @MichaReiser on 2025-04-27 10:01_

> Also, apologies for the late review, falling a bit behind on notifications.

No worries. I think you prioritized correctly. This PR isn't urgent.

---

_@MichaReiser reviewed on 2025-04-27 10:01_

---

_Review comment by @MichaReiser on `crates/ruff_source_file/src/line_index.rs`:320 on 2025-04-27 10:01_

That's a great point

---

_Merged by @MichaReiser on 2025-04-27 10:27_

---

_Closed by @MichaReiser on 2025-04-27 10:27_

---

_Branch deleted on 2025-04-27 10:27_

---

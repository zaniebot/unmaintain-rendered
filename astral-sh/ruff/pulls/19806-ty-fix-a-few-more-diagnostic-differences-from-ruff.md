```yaml
number: 19806
title: "[ty] Fix a few more diagnostic differences from Ruff"
type: pull_request
state: merged
author: ntBre
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: brent/more-diag-fixes
created_at: 2025-08-07T13:18:26Z
updated_at: 2025-08-08T15:31:21Z
url: https://github.com/astral-sh/ruff/pull/19806
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix a few more diagnostic differences from Ruff

---

_Pull request opened by @ntBre on 2025-08-07 13:18_

## Summary

Fixes the remaining range reporting differences between the `ruff_db` diagnostic rendering and Ruff's existing rendering, as noted in https://github.com/astral-sh/ruff/pull/19415#issuecomment-3160525595.

This PR is structured as a series of three pairs. The first commit in each pair adds a test showing the previous behavior, followed by a fix and the updated snapshot. It's quite a small PR, but that might be helpful just for the contrast.

You can also look at [this range](https://github.com/astral-sh/ruff/pull/19415/files/052e656c6c1bd7586cbf97f049415d7ace08c631..c3ea51030d38d481aa10855ca1d2556e391ff4cb) of commits from #19415 to see the impact on real Ruff diagnostics. I spun these commits out of that PR.

## Test Plan

New `ruff_db` tests


---

_Comment by @github-actions[bot] on 2025-08-07 13:32_

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

_Marked ready for review by @ntBre on 2025-08-07 13:32_

---

_Review requested from @carljm by @ntBre on 2025-08-07 13:33_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-07 13:33_

---

_Review requested from @sharkdp by @ntBre on 2025-08-07 13:33_

---

_Review requested from @dcreager by @ntBre on 2025-08-07 13:33_

---

_Review requested from @BurntSushi by @ntBre on 2025-08-07 13:33_

---

_Review request for @dcreager removed by @ntBre on 2025-08-07 13:33_

---

_Review request for @carljm removed by @ntBre on 2025-08-07 13:33_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-07 13:33_

---

_Label `ty` added by @MichaReiser on 2025-08-07 13:34_

---

_Label `diagnostics` added by @MichaReiser on 2025-08-07 13:34_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1279 on 2025-08-07 13:35_

I'd find a comment what's happening here useful :) Together with a comment that this is another upstream divergence

---

_Renamed from "Fix a few more diagnostic differences from Ruff" to "[ty] Fix a few more diagnostic differences from Ruff" by @ntBre on 2025-08-07 13:35_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-07 13:36_

Is the `+1` because the line is zero indexed or because we want to point to the next line. 

I think it would also be okay to say `last_line:after_last_column` which would better align with how the snippet is rendered.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:1020 on 2025-08-07 13:41_

What I like doing here is `&source[index + 1..].starts_width('\n')`

---

_@MichaReiser approved on 2025-08-07 13:42_

Nice!

My only suggestion is to use `last-line:last-column` for the eof case instead of `last_line+1:1` to more closely match the rendered snippet

---

_@ntBre reviewed on 2025-08-07 13:53_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-07 13:53_

Yes, the +1 is to point to the next line to match Ruff's current behavior:

https://github.com/astral-sh/ruff/blob/c401a6d86e2102f10ae5dc933e3daf723536a6b3/crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC001_ISC_syntax_error.py.snap#L173-L178

I agree with you that 29:2 would make sense and more closely align with the rendering, though.

One issue I ran into here was that (naively) computing the column breaks some other annotate-snippets tests. Maybe that's a sign that it's not the right fix.

I'll look at this a bit more.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:665 on 2025-08-07 13:54_

Very very minor, but it might be nice if we had a `TextSize::ZERO` constant or something. Using `default()` for this feels a little funny.

---

_@ntBre reviewed on 2025-08-07 13:54_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1279 on 2025-08-07 13:54_

Yeah definitely makes sense to put the comment here, rather than on the test!

---

_@BurntSushi approved on 2025-08-07 13:58_

---

_@MichaReiser reviewed on 2025-08-07 14:03_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:665 on 2025-08-07 14:03_

You can do `TextSize::new(0)` but agree that `ZERO` would be nice`

---

_@ntBre reviewed on 2025-08-07 14:08_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:665 on 2025-08-07 14:08_

Added! I think `ONE` might be helpful too, but not in this PR.

---

_@ntBre reviewed on 2025-08-07 14:36_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-07 14:36_

I have a more robust fix working, but this will also be a mismatch from the `concise` rendering. Should I update that as well? It looks like those numbers come from our `LineIndex` code.

---

_@MichaReiser reviewed on 2025-08-07 15:48_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-07 15:48_

Fixing `LineIndex` does sound reasonable if that's where the numbers are coming from. But I don't have a good sense of the blast radius. You might have to try to see what changes (Updating `concise` makes sense to me, we want the line numbers to match across formats)

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-07 16:29_

Yeah I definitely want them to match, I was more wondering if you'd rather align on this new behavior or preserve the old behavior. I'll have to double check the other output formats too. Hopefully they all use the line index. I guess `full` is the only case where the output is noticeably questionable with the caret on a different line from what the range reports.

---

_@ntBre reviewed on 2025-08-07 16:29_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-07 16:40_

I'd prefer aligning on the new behavior. I think the existing behavior is even confusing in the context of the new newline at end of file because it suggests that there's a newline (which obviously there isn't)

---

_@MichaReiser reviewed on 2025-08-07 16:40_

---

_@ntBre reviewed on 2025-08-08 15:18_

---

_Review comment by @ntBre on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-08 15:18_

For future reference, we discussed this on Discord and decided to move ahead with preserving the old behavior for now. This seems like the same, or a closely-related, issue as https://github.com/astral-sh/ruff/issues/15510, so we can follow-up on resolving the header-rendering mismatch separately. I added some notes there (https://github.com/astral-sh/ruff/issues/15510#issuecomment-3168270151) from looking into it today too.

---

_@MichaReiser reviewed on 2025-08-08 15:19_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1288 on 2025-08-08 15:19_

Perfect. Thank you for looking into it so carefully! Let's merge :)

---

_Merged by @ntBre on 2025-08-08 15:31_

---

_Closed by @ntBre on 2025-08-08 15:31_

---

_Branch deleted on 2025-08-08 15:31_

---

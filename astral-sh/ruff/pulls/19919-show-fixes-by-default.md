```yaml
number: 19919
title: Show fixes by default
type: pull_request
state: merged
author: ntBre
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: brent/show-fixes
created_at: 2025-08-14T18:52:50Z
updated_at: 2025-08-29T13:53:07Z
url: https://github.com/astral-sh/ruff/pull/19919
synced_at: 2026-01-10T17:46:21Z
```

# Show fixes by default

---

_Pull request opened by @ntBre on 2025-08-14 18:52_

## Summary

This PR fixes #7352 by exposing the `show_fix_diff` option used in our snapshot tests in the CLI. As the issue suggests, we plan to make this the default output format in the future, so this is added to the `full` output format in preview for now.

This turned out to be pretty straightforward. I just used our existing `Applicability` settings to determine whether or not to print the diff. 

The snapshot differences are because we now set `Applicability::DisplayOnly` for our snapshot tests. This `Applicability` is also used to determine whether or not the fix icon (`[*]`) is rendered, so this is now shown for display-only fixes in our snapshots. This was already the case previously, but we were only setting `Applicability::Unsafe` in these tests and ignoring the `Applicability` when rendering fix diffs. CLI users can't enable display-only fixes, so this is only a test change for now, but this should work smoothly if we decide to expose a `--display-only-fixes` flag or similar in the future.

I also deleted the `PrinterFlags::SHOW_FIX_DIFF` flag. This was completely unused before, and it seemed less confusing just to delete it than to enable it in the right place and check it along with the `OutputFormat` and `preview`.

## Test Plan

I only added one CLI test for now. I'm kind of assuming that we have decent coverage of the cases where this shouldn't be firing, especially the `output_format` CLI test, which shows that this definitely doesn't affect non-preview `full` output. I'm happy to add more tests with different combinations of options, if we're worried about any in particular. I did try `--diff` and `--preview` and a few other combinations manually.

And here's a screenshot using our trusty UP049 example from the design discussion confirming that all the colors and other formatting still look as expected:

<img width="786" height="629" alt="image" src="https://github.com/user-attachments/assets/94e408bc-af7b-4573-b546-a5ceac2620f2" />

And one with an unsafe fix to see the footer:

<img width="782" height="367" alt="image" src="https://github.com/user-attachments/assets/bbb29e47-310b-4293-b2c2-cc7aee3baff4" />


## Related issues and PR
- https://github.com/astral-sh/ruff/issues/7352
- https://github.com/astral-sh/ruff/pull/12595
- https://github.com/astral-sh/ruff/issues/12598
- https://github.com/astral-sh/ruff/issues/12599
- https://github.com/astral-sh/ruff/issues/12600

I think we could probably close all of these issues now. I think we've either resolved or avoided most of them, and if we encounter them again with the new output format, it would probably make sense to open new ones anyway.

---

_Label `cli` added by @ntBre on 2025-08-14 18:52_

---

_Label `preview` added by @ntBre on 2025-08-14 18:52_

---

_Comment by @github-actions[bot] on 2025-08-14 19:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[prototype] Show fixes by default" to "Show fixes by default" by @ntBre on 2025-08-28 17:42_

---

_Marked ready for review by @ntBre on 2025-08-28 19:35_

---

_Review requested from @carljm by @ntBre on 2025-08-28 19:35_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-28 19:35_

---

_Review requested from @sharkdp by @ntBre on 2025-08-28 19:35_

---

_Review requested from @dcreager by @ntBre on 2025-08-28 19:35_

---

_Review requested from @AlexWaygood by @ntBre on 2025-08-28 19:35_

---

_Review request for @dcreager removed by @ntBre on 2025-08-28 19:36_

---

_Review request for @carljm removed by @ntBre on 2025-08-28 19:36_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-28 19:36_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-08-28 19:36_

---

_Review requested from @zanieb by @ntBre on 2025-08-28 19:36_

---

_Review requested from @dhruvmanila by @ntBre on 2025-08-28 19:36_

---

_@zanieb approved on 2025-08-28 19:57_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/diagnostic/mod.rs`:354 on 2025-08-29 08:21_

nit: `fix_applicable` / `is_fix_applicable`

---

_@dhruvmanila approved on 2025-08-29 08:27_

---

_@zanieb reviewed on 2025-08-29 13:24_

---

_Review comment by @zanieb on `crates/ruff_db/src/diagnostic/mod.rs`:354 on 2025-08-29 13:24_

I was also thinking about this but landed on not saying something :) I think my best idea was `has_applicable_fix`

---

_@zanieb reviewed on 2025-08-29 13:25_

---

_Review comment by @zanieb on `crates/ruff_db/src/diagnostic/mod.rs`:354 on 2025-08-29 13:25_

(I think the `is_applicable` verbiage makes more sense on the fix type)

---

_Review comment by @zanieb on `crates/ruff_db/src/diagnostic/mod.rs`:354 on 2025-08-29 13:26_

(@ntBre un-resolving because I commented as you were resolving. Just for visibility — I don't have strong feelings)

---

_@zanieb reviewed on 2025-08-29 13:26_

---

_@ntBre reviewed on 2025-08-29 13:27_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:354 on 2025-08-29 13:27_

Ah `diagnostic.has_applicable_fix` does read nicely, thanks!

---

_Merged by @ntBre on 2025-08-29 13:53_

---

_Closed by @ntBre on 2025-08-29 13:53_

---

_Branch deleted on 2025-08-29 13:53_

---

```yaml
number: 20595
title: "Remove `TextEmitter`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/remove-text-emitter
created_at: 2025-09-26T15:46:10Z
updated_at: 2025-09-29T12:46:27Z
url: https://github.com/astral-sh/ruff/pull/20595
synced_at: 2026-01-10T17:40:28Z
```

# Remove `TextEmitter`

---

_Pull request opened by @ntBre on 2025-09-26 15:46_

## Summary

Addresses https://github.com/astral-sh/ruff/pull/20443#discussion_r2381237640 by factoring out the `match` on the ruff output format in a way that should be reusable by the formatter.

I didn't think this was going to work at first, but the fact that the config holds options that apply only to certain output formats works in our favor here. We can set up a single config for all of the output formats and then use `try_from` to convert the `OutputFormat` to a `DiagnosticFormat` later.

## Test Plan

Existing tests, plus a few new ones to make sure relocating the `SHOW_FIX_SUMMARY` rendering worked, that was untested before. I deleted a bunch of test code along with the `text` module, but I believe all of it is now well-covered by the `full` and `concise` tests in `ruff_db`.

I also merged this branch into https://github.com/astral-sh/ruff/pull/20443 locally and made sure that the API actually helps. `render_diagnostics` dropped in perfectly and passed the tests there too.


---

_Label `internal` added by @ntBre on 2025-09-26 15:46_

---

_Comment by @github-actions[bot] on 2025-09-26 15:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-09-26 16:54_

---

_Review requested from @carljm by @ntBre on 2025-09-26 16:54_

---

_Review requested from @MichaReiser by @ntBre on 2025-09-26 16:54_

---

_Review requested from @sharkdp by @ntBre on 2025-09-26 16:54_

---

_Review requested from @dcreager by @ntBre on 2025-09-26 16:54_

---

_Review request for @dcreager removed by @ntBre on 2025-09-26 16:54_

---

_Review request for @carljm removed by @ntBre on 2025-09-26 16:54_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-26 16:54_

---

_@MichaReiser approved on 2025-09-29 07:14_

---

_Merged by @ntBre on 2025-09-29 12:46_

---

_Closed by @ntBre on 2025-09-29 12:46_

---

_Branch deleted on 2025-09-29 12:46_

---

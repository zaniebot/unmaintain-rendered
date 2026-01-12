```yaml
number: 20320
title: "Move GitHub rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/github
created_at: 2025-09-09T20:07:20Z
updated_at: 2025-09-11T17:11:16Z
url: https://github.com/astral-sh/ruff/pull/20320
synced_at: 2026-01-12T15:56:59Z
```

# Move GitHub rendering to `ruff_db`

---

_@ntBre_

## Summary

This is the GitHub analog to #20117. This PR prepares to add a GitHub output format to ty by moving the implementation from `ruff_linter` to `ruff_db`. Hopefully this one is a bit easier to review commit-by-commit. Almost all of the refactoring this time is in the first commit, then the second commit adds the new `OutputFormat` variant and moves the file into `ruff_db`. The third commit is just a small touch up to use a private method that accommodates ty files so that we can run the tests and update/move the snapshots.

I had to push a fourth commit to fix and test diagnostics without a span/file.

## Test Plan

Existing tests


---

_Label `internal` added by @ntBre on 2025-09-09 20:07_

---

_Label `diagnostics` added by @ntBre on 2025-09-09 20:07_

---

_Comment by @github-actions[bot] on 2025-09-09 20:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-09-10 19:38_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/github.rs`:51 on 2025-09-10 19:38_

I think we could omit these entirely if there's no range, based on the [docs](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-commands#setting-an-error-message), but I stuck with `unwrap_or_default` for now since that's what we were already doing for notebooks.

---

_Marked ready for review by @ntBre on 2025-09-10 19:55_

---

_Review requested from @carljm by @ntBre on 2025-09-10 19:55_

---

_Review requested from @MichaReiser by @ntBre on 2025-09-10 19:55_

---

_Review requested from @sharkdp by @ntBre on 2025-09-10 19:55_

---

_Review requested from @dcreager by @ntBre on 2025-09-10 19:55_

---

_Review request for @dcreager removed by @ntBre on 2025-09-10 19:55_

---

_Review request for @carljm removed by @ntBre on 2025-09-10 19:55_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-10 19:55_

---

_Review requested from @BurntSushi by @ntBre on 2025-09-10 19:55_

---

_@BurntSushi approved on 2025-09-11 12:30_

Nice! The commit breakdown was perfect, thank you!

I like the unwrap/expect removals. :-)

---

_Merged by @ntBre on 2025-09-11 17:11_

---

_Closed by @ntBre on 2025-09-11 17:11_

---

_Branch deleted on 2025-09-11 17:11_

---

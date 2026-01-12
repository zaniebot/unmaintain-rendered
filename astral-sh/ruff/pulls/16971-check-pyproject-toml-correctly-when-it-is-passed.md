```yaml
number: 16971
title: "Check `pyproject.toml` correctly when it is passed via stdin"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - cli
assignees: []
merged: true
base: main
head: pyproject-toml-stdin
created_at: 2025-03-25T16:27:27Z
updated_at: 2025-03-27T16:41:00Z
url: https://github.com/astral-sh/ruff/pull/16971
synced_at: 2026-01-12T15:55:59Z
```

# Check `pyproject.toml` correctly when it is passed via stdin

---

_@InSyncWithFoo_

## Summary

Resolves #16950 and [a 1.5-year-old TODO comment](https://github.com/astral-sh/ruff/blame/8d16a5c8c98089b0d1be3e3fd68954be62dd2598/crates/ruff/src/diagnostics.rs#L380).

After this change, a `pyproject.toml` will be linted the same as any Python files would when passed via stdin.

## Test Plan

Integration tests.


---

_Comment by @github-actions[bot] on 2025-03-25 16:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `cli` added by @MichaReiser on 2025-03-27 11:39_

---

_@MichaReiser reviewed on 2025-03-27 11:41_

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:393 on 2025-03-27 11:41_

We should only call `lint_pyproject_toml` if the user has any pyproject.toml related rule enabled, similar to 

https://github.com/astral-sh/ruff/blob/74f64d3f96d76fc252512acf30cf45b252be58f7/crates/ruff/src/diagnostics.rs#L229-L239

Could we share some of the logic between `lint_path` and `lint_stdin`

---

_Review comment by @InSyncWithFoo on `crates/ruff/src/diagnostics.rs`:393 on 2025-03-27 15:20_

There are so few similarities I don't think it's worth it to share them.

---

_@InSyncWithFoo reviewed on 2025-03-27 15:20_

---

_@MichaReiser reviewed on 2025-03-27 16:01_

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:393 on 2025-03-27 16:01_

I think there's definitely an opportunity to share more but I can see how it is out of the scope for this PR

---

_Merged by @MichaReiser on 2025-03-27 16:01_

---

_Closed by @MichaReiser on 2025-03-27 16:01_

---

_Branch deleted on 2025-03-27 16:41_

---

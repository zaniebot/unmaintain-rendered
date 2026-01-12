```yaml
number: 9095
title: Hide unsafe fix suggestions when explicitly disabled
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/unsafe-hint-hide
created_at: 2023-12-11T16:04:03Z
updated_at: 2023-12-11T21:43:04Z
url: https://github.com/astral-sh/ruff/pull/9095
synced_at: 2026-01-10T23:40:55Z
```

# Hide unsafe fix suggestions when explicitly disabled

---

_Pull request opened by @zanieb on 2023-12-11 16:04_

Hides hints about unsafe fixes when they are disabled e.g. with `--no-unsafe-fixes` or `unsafe-fixes = false`. By default, unsafe fix hints are still displayed. This seems like a nice way to remove the nag for users who have chosen not to apply unsafe fixes.

Inspired by comment at https://github.com/astral-sh/ruff/issues/9063#issuecomment-1850289675

---

_Review requested from @charliermarsh by @zanieb on 2023-12-11 16:10_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/printer.rs`:159 on 2023-12-11 16:12_

nit: maybe combine the separate `else` and `if` into a single `else if`?

---

_@dhruvmanila approved on 2023-12-11 16:14_

Nice!

---

_Comment by @github-actions[bot] on 2023-12-11 16:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2023-12-11 16:29_

---

_Review comment by @zanieb on `crates/ruff_cli/src/printer.rs`:159 on 2023-12-11 16:29_

Hm.. I think the intent here is clearer as written as the `else` is for `UnsafeFixes` modes.

---

_@konstin approved on 2023-12-11 16:32_

---

_Merged by @zanieb on 2023-12-11 21:42_

---

_Closed by @zanieb on 2023-12-11 21:42_

---

_Branch deleted on 2023-12-11 21:42_

---

_Label `cli` added by @zanieb on 2023-12-11 21:43_

---

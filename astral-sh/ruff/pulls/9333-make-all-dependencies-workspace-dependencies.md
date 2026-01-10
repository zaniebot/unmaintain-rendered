```yaml
number: 9333
title: Make all dependencies workspace dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/workspace
created_at: 2023-12-31T15:38:22Z
updated_at: 2024-01-04T14:31:54Z
url: https://github.com/astral-sh/ruff/pull/9333
synced_at: 2026-01-10T23:07:18Z
```

# Make all dependencies workspace dependencies

---

_Pull request opened by @charliermarsh on 2023-12-31 15:38_

## Summary

This PR modifies our `Cargo.toml` files to use workspace dependencies for _all_ dependencies, rather than the status quo of sporadically trying to use workspace dependencies for those dependencies that are used across multiple crates. I find the current situation more confusing and harder to manage, since we have a mix of workspace and crate-local dependencies, whereas this setup consistently uses the same approach for all dependencies.


---

_Review requested from @BurntSushi by @charliermarsh on 2023-12-31 15:38_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-12-31 15:38_

---

_Label `internal` added by @charliermarsh on 2023-12-31 15:38_

---

_Comment by @github-actions[bot] on 2023-12-31 15:54_

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

_Comment by @konstin on 2024-01-02 09:43_

Big :+1: keeping all the `[dependencies]` tables consistent. One disadvantage is that this pulls `[dev-dependencies]` and `ruff_dev` into `[workspace.dependencies]`, so the top level dependency table contains crates that the `ruff` binary doesn't require, e.g. `tempfile` and `imara-diff` (that's not our diff library, we use `similar`, only `ruff_dev` uses `imara-diff` for perf reasons).

---

_@konstin approved on 2024-01-02 09:43_

---

_Comment by @charliermarsh on 2024-01-02 13:28_

👍 Makes sense -- maybe I'll move `ruff_dev` to use non-workspace dependencies for everything, actually.

---

_Comment by @charliermarsh on 2024-01-02 13:40_

Hmm, I started to do this (move all dev dependencies out of the workspace) but then kinda changed my mind. It still seems good to have them use consistent declarations.

---

_Merged by @charliermarsh on 2024-01-02 13:42_

---

_Closed by @charliermarsh on 2024-01-02 13:42_

---

_Branch deleted on 2024-01-02 13:42_

---

_@BurntSushi reviewed on 2024-01-04 14:31_

I like this. I was also unsure of when to add something as a workspace dep and when not to.

---

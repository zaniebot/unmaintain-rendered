```yaml
number: 8733
title: Factor out a builder to handle common integration test arguments
type: pull_request
state: merged
author: alanhdu
labels:
  - internal
assignees: []
merged: true
base: main
head: alan/integration
created_at: 2023-11-17T02:22:32Z
updated_at: 2023-11-22T00:38:26Z
url: https://github.com/astral-sh/ruff/pull/8733
synced_at: 2026-01-10T23:40:55Z
```

# Factor out a builder to handle common integration test arguments

---

_Pull request opened by @alanhdu on 2023-11-17 02:22_

## Summary

This refactors the `ruff_cli` integration tests to create a new `RuffCheck` struct -- this holds options to configure the "common case" flags that we want to pass to Ruff (e.g. `--no-cache`, `--isolated`, etc). This helps reduce the boilerplate and (IMO) makes it more obvious what the core logic of each test is by keeping only the "interesting" parameters.

## Test Plan

`cargo test`


---

_Comment by @alanhdu on 2023-11-17 02:23_

I tried to go for something that's simple to write and use and avoid overly-abstracting what's going on here, but am totally open to other approaches here.

---

_Comment by @github-actions[bot] on 2023-11-18 16:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-22 00:17_

Thanks!

---

_Label `internal` added by @charliermarsh on 2023-11-22 00:18_

---

_Merged by @charliermarsh on 2023-11-22 00:32_

---

_Closed by @charliermarsh on 2023-11-22 00:32_

---

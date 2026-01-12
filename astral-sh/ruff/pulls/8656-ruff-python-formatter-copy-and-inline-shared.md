```yaml
number: 8656
title: "ruff_python_formatter: copy and inline shared traits"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fmt/inline-shared-traits
created_at: 2023-11-13T16:56:44Z
updated_at: 2023-11-13T17:16:05Z
url: https://github.com/astral-sh/ruff/pull/8656
synced_at: 2026-01-10T23:40:55Z
```

# ruff_python_formatter: copy and inline shared traits

---

_Pull request opened by @BurntSushi on 2023-11-13 16:56_

It seems as though using `include!(...)` to avoid the source code copy breaks rust-analzer. Namely, it treats the included file as unlinked, and so any part of analysis (e.g., goto-definition) that needs that file to reason about the code ends up failing.

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-13 16:56_

---

_@charliermarsh reviewed on 2023-11-13 16:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/lib.rs`:38 on 2023-11-13 16:58_

I'd suggest removing this line entirely.

---

_@charliermarsh approved on 2023-11-13 16:58_

---

_Label `internal` added by @charliermarsh on 2023-11-13 17:00_

---

_@BurntSushi reviewed on 2023-11-13 17:01_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/lib.rs`:38 on 2023-11-13 17:01_

Ah yeah derp. That was an accident.

---

_Comment by @github-actions[bot] on 2023-11-13 17:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @BurntSushi on 2023-11-13 17:16_

---

_Closed by @BurntSushi on 2023-11-13 17:16_

---

_Branch deleted on 2023-11-13 17:16_

---

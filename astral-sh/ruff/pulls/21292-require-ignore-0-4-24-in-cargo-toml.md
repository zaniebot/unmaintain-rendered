```yaml
number: 21292
title: "Require ignore 0.4.24 in `Cargo.toml`"
type: pull_request
state: merged
author: musicinmybrain
labels: []
assignees: []
merged: true
base: main
head: ignore-0.4.24
created_at: 2025-11-06T12:37:05Z
updated_at: 2025-11-06T22:45:16Z
url: https://github.com/astral-sh/ruff/pull/21292
synced_at: 2026-01-12T15:57:20Z
```

# Require ignore 0.4.24 in `Cargo.toml`

---

_@musicinmybrain_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Since 4c4ddc8c29e, ruff uses the `WalkBuilder::current_dir` API [introduced in `ignore` version 0.4.24](https://diff.rs/ignore/0.4.23/0.4.24/src%2Fwalk.rs), so it should explicitly depend on this minimum version.

See also https://github.com/astral-sh/ruff/pull/20979.

## Test Plan

<!-- How was it tested? -->
Source inspection verifies this version is necessary; no additional testing is required since `Cargo.lock` already has (at least) this version.

---

_@charliermarsh approved on 2025-11-06 12:43_

---

_Merged by @charliermarsh on 2025-11-06 12:43_

---

_Closed by @charliermarsh on 2025-11-06 12:43_

---

_Comment by @github-actions[bot] on 2025-11-06 12:48_

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

_Comment by @T-256 on 2025-11-06 22:45_

I wonder if there is anyway to prevent this misconfiguration in future. IOW, could test against minimum declared dependencies versions set in `Cargo.toml`?

---

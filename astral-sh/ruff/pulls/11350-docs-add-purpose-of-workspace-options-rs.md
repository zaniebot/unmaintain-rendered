```yaml
number: 11350
title: "docs: Add purpose of workspace/options.rs"
type: pull_request
state: closed
author: Abdur-rahmaanJ
labels: []
assignees: []
base: main
head: patch-3
created_at: 2024-05-09T10:59:50Z
updated_at: 2024-05-09T17:39:26Z
url: https://github.com/astral-sh/ruff/pull/11350
synced_at: 2026-01-12T15:55:37Z
```

# docs: Add purpose of workspace/options.rs

---

_@Abdur-rahmaanJ_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

State the difference between options.rs and workspace.rs

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "docs: Add purpose of workspace/option.rs" to "docs: Add purpose of workspace/options.rs" by @Abdur-rahmaanJ on 2024-05-09 11:09_

---

_Comment by @github-actions[bot] on 2024-05-09 11:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-05-09 13:44_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:1 on 2024-05-09 13:44_

This isn't quite right. These are the raw options that are read from `pyproject.toml` or `ruff.toml`. They're defined within Ruff and passed to the various packages that rely on them (e.g., our isort implementation).

We then have `Configuration` which is an intermediate representation, and `Settings` which is the final internal representation that _actually_ powers Ruff. But `Options` need to match the TOML schema _exactly_.

---

_@Abdur-rahmaanJ reviewed on 2024-05-09 17:39_

---

_Review comment by @Abdur-rahmaanJ on `crates/ruff_workspace/src/options.rs`:1 on 2024-05-09 17:39_

Oh lol thanks i was wondering how weird what do you need isort configs for. will rephrase

---

_Closed by @Abdur-rahmaanJ on 2024-05-09 17:39_

---

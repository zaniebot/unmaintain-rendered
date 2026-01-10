```yaml
number: 8276
title: "Exclude directories with `tool.exclude`"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: tool-exclude-directory-exclusion
created_at: 2023-10-27T09:42:38Z
updated_at: 2024-02-29T11:08:23Z
url: https://github.com/astral-sh/ruff/pull/8276
synced_at: 2026-01-10T22:47:01Z
```

# Exclude directories with `tool.exclude`

---

_Pull request opened by @MichaReiser on 2023-10-27 09:42_

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

_Comment by @MichaReiser on 2023-10-27 09:42_

Current dependencies on/for this PR:
* main
  * **PR #8276** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8276" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8276?utm_source=stack-comment).

---

_@zanieb reviewed on 2023-10-27 20:51_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/.format.rs.pending-snap`:1 on 2023-10-27 20:51_

Oopsies?

---

_Comment by @charliermarsh on 2023-10-28 11:18_

If we _don't_ change this, we do need to update the documentation for these settings, which suggests `Single-path patterns, like .mypy_cache (to exclude any directory named .mypy_cache in the tree)`.

---

_Comment by @github-actions[bot] on 2023-11-03 09:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Closed by @MichaReiser on 2024-02-29 11:08_

---

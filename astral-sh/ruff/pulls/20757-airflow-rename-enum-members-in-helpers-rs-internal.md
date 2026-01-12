```yaml
number: 20757
title: "[`airflow`]: Rename Enum members in helpers.rs (`internal`)"
type: pull_request
state: closed
author: Lee-W
labels: []
assignees: []
draft: true
base: main
head: fix-comments
created_at: 2025-10-07T23:44:22Z
updated_at: 2025-12-10T17:41:18Z
url: https://github.com/astral-sh/ruff/pull/20757
synced_at: 2026-01-12T15:57:09Z
```

# [`airflow`]: Rename Enum members in helpers.rs (`internal`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

* Rename the following enum members
    * `Rename` → `SymbolRenamed` 
    * `SourceModuleMoved` → `SymbolsMoved`
    * `AttrName` → `AttrRenamed`
* misc
    * fix wrong comments 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-10-07 23:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "docs(airflow): fix wrong comments in utility functions" to "[`airflow`]: Rename Enum members in helpers.rs (`internal`)" by @Lee-W on 2025-10-14 10:27_

---

_Comment by @MichaReiser on 2025-12-10 17:41_

Thanks for your contribution. I'll close this PR because it has become stale. Please don't hesitate to resubmit the PR and reference this PR if you plan to keep working on this feature.

---

_Closed by @MichaReiser on 2025-12-10 17:41_

---

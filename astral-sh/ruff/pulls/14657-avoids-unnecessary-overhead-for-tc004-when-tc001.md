```yaml
number: 14657
title: "Avoids unnecessary overhead for `TC004`, when `TC001-003` are disabled"
type: pull_request
state: merged
author: Daverball
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor/unnecessary-tc004-overhead
created_at: 2024-11-28T15:02:13Z
updated_at: 2024-11-28T15:28:38Z
url: https://github.com/astral-sh/ruff/pull/14657
synced_at: 2026-01-10T20:50:57Z
```

# Avoids unnecessary overhead for `TC004`, when `TC001-003` are disabled

---

_Pull request opened by @Daverball on 2024-11-28 15:02_

## Summary

Another minor thing that came up as part of #12927

TC004 doesn't use the `runtime_imports` vector we're building during analysis of deferred scopes, so we don't need to do that work when it's the only typing import rule that is enabled.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-11-28 15:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-11-28 15:28_

Thanks

---

_Label `internal` added by @MichaReiser on 2024-11-28 15:28_

---

_Merged by @MichaReiser on 2024-11-28 15:28_

---

_Closed by @MichaReiser on 2024-11-28 15:28_

---

_Branch deleted on 2024-11-28 15:28_

---

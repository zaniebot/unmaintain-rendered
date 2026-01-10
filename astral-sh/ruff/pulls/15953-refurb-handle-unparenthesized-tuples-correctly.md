```yaml
number: 15953
title: "[`refurb`] Handle unparenthesized tuples correctly (`FURB122`, `FURB142`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: FURB122
created_at: 2025-02-05T00:07:50Z
updated_at: 2025-02-05T17:05:59Z
url: https://github.com/astral-sh/ruff/pull/15953
synced_at: 2026-01-10T19:57:22Z
```

# [`refurb`] Handle unparenthesized tuples correctly (`FURB122`, `FURB142`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-05 00:07_

## Summary

Resolves #15936.

The fixes will now attempt to preserve the original iterable's format and quote it if necessary. For `FURB142`, comments within the fix range will make it unsafe as well.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-05 00:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-02-05 09:14_

---

_Label `fixes` added by @MichaReiser on 2025-02-05 09:14_

---

_@MichaReiser approved on 2025-02-05 09:16_

Nice

---

_Merged by @MichaReiser on 2025-02-05 09:16_

---

_Closed by @MichaReiser on 2025-02-05 09:16_

---

_Branch deleted on 2025-02-05 17:05_

---

```yaml
number: 15409
title: "[red-knot] Understand `type[Unknown]`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-type-unknown
created_at: 2025-01-10T21:06:56Z
updated_at: 2025-01-16T12:48:42Z
url: https://github.com/astral-sh/ruff/pull/15409
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Understand `type[Unknown]`

---

_@InSyncWithFoo_

## Summary

Follow-up to #15194.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-10 21:06_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-10 21:06_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-10 21:06_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-10 21:06_

---

_Comment by @github-actions[bot] on 2025-01-10 21:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2025-01-10 21:25_

Thank you! I was going to comment on the previous PR that this should be really easy to fix, but it also seemed like scope creep for that PR. Thanks for following up on it.

---

_Comment by @carljm on 2025-01-10 21:25_

I'm not sure what is going on with the fuzzer build here, but it doesn't seem at all related to this PR.

---

_Merged by @carljm on 2025-01-10 21:25_

---

_Closed by @carljm on 2025-01-10 21:26_

---

_Branch deleted on 2025-01-10 21:26_

---

_Comment by @InSyncWithFoo on 2025-01-10 21:26_

@carljm `winnow` just got [a new release](https://github.com/winnow-rs/winnow/commit/dd81d34df05e37b52d8a2080bd45c346f5b46d1c). Apparently that contains some breaking changes.

---

_Label `red-knot` added by @dhruvmanila on 2025-01-16 12:48_

---

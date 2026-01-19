```yaml
number: 22729
title: "[ty] Allow `if type(x) is Y` narrowing for types other than class-literal types"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/type-t-narrow
created_at: 2026-01-19T17:25:04Z
updated_at: 2026-01-19T17:25:05Z
url: https://github.com/astral-sh/ruff/pull/22729
synced_at: 2026-01-19T17:26:16Z
```

# [ty] Allow `if type(x) is Y` narrowing for types other than class-literal types

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2565.

This PR adds support for `if type(x) is Y` narrowing where `Y` is a subclass-of type, type-alias type, or typevar type.

## Test Plan

mdtests


---

_Review requested from @carljm by @AlexWaygood on 2026-01-19 17:25_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-19 17:25_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-19 17:25_

---

_Label `ty` added by @AlexWaygood on 2026-01-19 17:25_

---

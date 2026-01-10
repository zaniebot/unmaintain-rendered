```yaml
number: 17741
title: "[red-knot] Ban direct instantiation of generic protocols as well as non-generic ones"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generic-protocol-instantiation
created_at: 2025-04-30T15:56:38Z
updated_at: 2025-04-30T16:01:29Z
url: https://github.com/astral-sh/ruff/pull/17741
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Ban direct instantiation of generic protocols as well as non-generic ones

---

_Pull request opened by @AlexWaygood on 2025-04-30 15:56_

## Summary

This PR fixes an oversight from https://github.com/astral-sh/ruff/pull/17597. On `main`, we currently emit a diagnostic for `MyNonGenericProtocol()` but not for `MyGenericProtocol[int]()`. (One is a `Type::ClassLiteral` in our model, the other is a `Type::GenericAlias`.)

## Test Plan

new mdtests added that fail on `main`.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-30 15:56_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-30 15:56_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-30 15:56_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-30 15:56_

---

_Comment by @github-actions[bot] on 2025-04-30 16:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-04-30 16:01_

---

_Closed by @AlexWaygood on 2025-04-30 16:01_

---

_Branch deleted on 2025-04-30 16:01_

---

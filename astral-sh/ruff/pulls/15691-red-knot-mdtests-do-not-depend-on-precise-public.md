```yaml
number: 15691
title: "[red-knot] MDTests: Do not depend on precise public-symbol type inference"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/mdtests-less-dependence-on-public-symbol-inference-specifics
created_at: 2025-01-23T13:46:32Z
updated_at: 2025-01-23T13:51:34Z
url: https://github.com/astral-sh/ruff/pull/15691
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] MDTests: Do not depend on precise public-symbol type inference

---

_Pull request opened by @sharkdp on 2025-01-23 13:46_

## Summary

Another small PR to focus #15674 solely on the relevant changes. This makes our Markdown tests less dependent on precise types of public symbols, without actually changing anything semantically in these tests.

Best reviewed using ignore-whitespace-mode.

## Test Plan

Tested these changes on `main` and on the branch from #15674.


---

_Label `red-knot` added by @sharkdp on 2025-01-23 13:46_

---

_Review requested from @carljm by @sharkdp on 2025-01-23 13:46_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-23 13:46_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-23 13:46_

---

_@AlexWaygood approved on 2025-01-23 13:48_

---

_Merged by @sharkdp on 2025-01-23 13:51_

---

_Closed by @sharkdp on 2025-01-23 13:51_

---

_Branch deleted on 2025-01-23 13:51_

---

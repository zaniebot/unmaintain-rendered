```yaml
number: 11966
title: "[red-knot] Move the vendored typeshed stubs to the module resolver crate"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: move-vendored-typeshed
created_at: 2024-06-21T13:32:36Z
updated_at: 2024-07-01T10:29:42Z
url: https://github.com/astral-sh/ruff/pull/11966
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Move the vendored typeshed stubs to the module resolver crate

---

_Pull request opened by @AlexWaygood on 2024-06-21 13:32_

## Summary

This PR moves the vendored typeshed stubs to the module resolver crate, so that the stubs can be used by the module resolver. Other changes are kept to a bare minimum.


---

_Label `red-knot` added by @AlexWaygood on 2024-06-21 13:32_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-21 13:32_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-21 13:32_

---

_@MichaReiser approved on 2024-06-21 13:34_

I think it should now be possible to remove the `zip` dependency from the `red_knot` crate

---

_Merged by @AlexWaygood on 2024-06-21 13:47_

---

_Closed by @AlexWaygood on 2024-06-21 13:47_

---

_Branch deleted on 2024-07-01 10:29_

---

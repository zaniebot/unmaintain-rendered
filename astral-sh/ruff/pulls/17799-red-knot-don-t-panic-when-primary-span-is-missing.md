```yaml
number: 17799
title: "[red-knot] Don't panic when `primary-span` is missing while panicking"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/dont-panic-while-panicking
created_at: 2025-05-02T20:09:43Z
updated_at: 2025-05-03T12:08:07Z
url: https://github.com/astral-sh/ruff/pull/17799
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Don't panic when `primary-span` is missing while panicking

---

_Pull request opened by @MichaReiser on 2025-05-02 20:09_

## Summary

Panicking in a `Drop` implementation when the program is unwinding results in terminating the processes. 

We shouldn't panick in this case because the diagnostic is very likely incomplete and shoudln't be submitted.


---

_Review requested from @carljm by @MichaReiser on 2025-05-02 20:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-02 20:09_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-02 20:09_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-02 20:09_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-05-02 20:09_

---

_Label `red-knot` added by @MichaReiser on 2025-05-02 20:09_

---

_@carljm approved on 2025-05-02 20:10_

---

_Closed by @MichaReiser on 2025-05-02 20:15_

---

_Reopened by @MichaReiser on 2025-05-02 20:15_

---

_Comment by @github-actions[bot] on 2025-05-02 20:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-05-02 20:19_

---

_Closed by @MichaReiser on 2025-05-02 20:19_

---

_Branch deleted on 2025-05-02 20:19_

---

_@BurntSushi reviewed on 2025-05-03 12:08_

Makes sense to me!

---

```yaml
number: 17500
title: "[red-knot] mypy_primer: Use upstream repo"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/mypy_primer-use-upstream-repo
created_at: 2025-04-20T17:35:10Z
updated_at: 2025-04-22T10:14:27Z
url: https://github.com/astral-sh/ruff/pull/17500
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] mypy_primer: Use upstream repo

---

_@sharkdp_

## Summary

Switch to the official version of [`mypy_primer`](https://github.com/hauntsaninja/mypy_primer), now that Red Knot support has been upstreamed (see https://github.com/hauntsaninja/mypy_primer/pull/138, https://github.com/hauntsaninja/mypy_primer/pull/135, https://github.com/hauntsaninja/mypy_primer/pull/151, https://github.com/hauntsaninja/mypy_primer/pull/155).

## Test Plan

Locally and in CI


---

_Label `ci` added by @sharkdp on 2025-04-20 17:35_

---

_Label `red-knot` added by @sharkdp on 2025-04-20 17:35_

---

_Comment by @github-actions[bot] on 2025-04-20 17:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-04-22 09:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/primer/good.txt`:1 on 2025-04-22 09:44_

This package is not included upstream. I used it for testing something, I don't think it's important that we keep it.

---

_@sharkdp reviewed on 2025-04-22 09:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/primer/good.txt`:21 on 2025-04-22 09:44_

I added this package in our fork, as it made use of `@dataclass_transform`. I'll propose to add this upstream.

---

_Marked ready for review by @sharkdp on 2025-04-22 09:52_

---

_Review requested from @carljm by @sharkdp on 2025-04-22 09:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-22 09:52_

---

_Review requested from @dcreager by @sharkdp on 2025-04-22 09:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-04-22 09:52_

---

_Merged by @sharkdp on 2025-04-22 09:55_

---

_Closed by @sharkdp on 2025-04-22 09:55_

---

_Branch deleted on 2025-04-22 09:55_

---

_@sharkdp reviewed on 2025-04-22 10:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/primer/good.txt`:21 on 2025-04-22 10:14_

https://github.com/hauntsaninja/mypy_primer/pull/157

---

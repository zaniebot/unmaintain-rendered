```yaml
number: 8419
title: "Add support for `ruff-ecosystem` format comparisons with `black`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/eco-cmp-black
created_at: 2023-11-01T18:45:52Z
updated_at: 2023-11-02T01:29:27Z
url: https://github.com/astral-sh/ruff/pull/8419
synced_at: 2026-01-10T23:40:55Z
```

# Add support for `ruff-ecosystem` format comparisons with `black`

---

_Pull request opened by @zanieb on 2023-11-01 18:45_

Extends https://github.com/astral-sh/ruff/pull/8416 activating the `black-and-ruff` and `black-then-ruff` formatter comparison modes for ecosystem checks allowing us to compare changes to Black across the ecosystem.

---

_Review requested from @konstin by @zanieb on 2023-11-01 18:46_

---

_Review requested from @MichaReiser by @zanieb on 2023-11-01 18:46_

---

_Label `internal` added by @zanieb on 2023-11-01 18:47_

---

_Comment by @github-actions[bot] on 2023-11-01 19:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_@zanieb reviewed on 2023-11-01 20:43_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:162 on 2023-11-01 20:43_

```suggestion
class RuffError(Exception):
```

---

_@zanieb reviewed on 2023-11-01 20:43_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:203 on 2023-11-01 20:43_

```suggestion
        raise RuffError(err.decode("utf8"))
```

---

_Marked ready for review by @zanieb on 2023-11-01 20:59_

---

_@MichaReiser approved on 2023-11-02 00:59_

---

_Merged by @zanieb on 2023-11-02 01:29_

---

_Closed by @zanieb on 2023-11-02 01:29_

---

_Branch deleted on 2023-11-02 01:29_

---

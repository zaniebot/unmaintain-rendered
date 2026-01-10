```yaml
number: 9411
title: Support variable keys in static dictionary key rule
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/B
created_at: 2024-01-06T20:35:48Z
updated_at: 2024-01-06T22:47:23Z
url: https://github.com/astral-sh/ruff/pull/9411
synced_at: 2026-01-10T22:57:09Z
```

# Support variable keys in static dictionary key rule

---

_Pull request opened by @charliermarsh on 2024-01-06 20:35_

Closes https://github.com/astral-sh/ruff/issues/9410.


---

_Label `rule` added by @charliermarsh on 2024-01-06 20:35_

---

_Merged by @charliermarsh on 2024-01-06 20:44_

---

_Closed by @charliermarsh on 2024-01-06 20:44_

---

_Branch deleted on 2024-01-06 20:44_

---

_Comment by @github-actions[bot] on 2024-01-06 20:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@mikaelarguedas reviewed on 2024-01-06 21:30_

---

_Review comment by @mikaelarguedas on `crates/ruff_linter/resources/test/fixtures/ruff/RUF011.py`:17 on 2024-01-06 21:30_

awesome :tada:, so quick :)

I dont know if this is a common use case but I guess one case not covered here is if the key in an attr.
e.g.
```python
{SomeClass.CONSTANT: val for val in data}
{Some.Class.CONSTANT: val for val in data}
```

---

_@charliermarsh reviewed on 2024-01-06 22:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/ruff/RUF011.py`:17 on 2024-01-06 22:09_

Good call, let me test that!

---

_@charliermarsh reviewed on 2024-01-06 22:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/ruff/RUF011.py`:17 on 2024-01-06 22:14_

See: https://github.com/astral-sh/ruff/pull/9416

---

_@mikaelarguedas reviewed on 2024-01-06 22:47_

---

_Review comment by @mikaelarguedas on `crates/ruff_linter/resources/test/fixtures/ruff/RUF011.py`:17 on 2024-01-06 22:47_

Awesome

---

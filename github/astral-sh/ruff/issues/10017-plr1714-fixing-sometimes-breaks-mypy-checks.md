---
number: 10017
title: PLR1714 fixing sometimes breaks mypy checks
type: issue
state: closed
author: RonnyPfannschmidt
labels:
  - bug
  - good first issue
  - rule
assignees: []
created_at: 2024-02-17T16:56:32Z
updated_at: 2024-02-20T19:03:34Z
url: https://github.com/astral-sh/ruff/issues/10017
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR1714 fixing sometimes breaks mypy checks

---

_Issue opened by @RonnyPfannschmidt on 2024-02-17 16:56_

as per https://github.com/pytest-dev/pytest/pull/11998

while i presume that most free-form uses should apply,
mypy cannot identify set membership checks for certain usages

so `sys.platform in {"win32", "emscripten"}` would trigger mypy confusion while ` sys.platform == "win32" or sys.platform == "emscripten"` would ensure mypy picks the correct stubs for each condition

a first naive heuristics could ensure the rule wont be apply to members of sys/platform


---

_Comment by @charliermarsh on 2024-02-17 18:03_

That’s a good idea. We should omit sys platform and Python version checks here.

---

_Label `bug` added by @charliermarsh on 2024-02-17 18:04_

---

_Label `good first issue` added by @charliermarsh on 2024-02-17 18:04_

---

_Label `rule` added by @AlexWaygood on 2024-02-18 11:31_

---

_Referenced in [astral-sh/ruff#10054](../../astral-sh/ruff/pulls/10054.md) on 2024-02-19 23:53_

---

_Closed by @charliermarsh on 2024-02-20 19:03_

---

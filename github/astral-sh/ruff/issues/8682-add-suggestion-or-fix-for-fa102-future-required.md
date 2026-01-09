---
number: 8682
title: Add suggestion or fix for FA102 (future-required-type-annotation)
type: issue
state: closed
author: RunDevelopment
labels:
  - fixes
assignees: []
created_at: 2023-11-14T18:48:33Z
updated_at: 2023-11-16T14:57:34Z
url: https://github.com/astral-sh/ruff/issues/8682
synced_at: 2026-01-07T13:12:15-06:00
---

# Add suggestion or fix for FA102 (future-required-type-annotation)

---

_Issue opened by @RunDevelopment on 2023-11-14 18:48_

The rule [FA102](https://docs.astral.sh/ruff/rules/future-required-type-annotation/) (future-required-type-annotation) is very useful when writing code in a code base that support python <3.10. The only issue with this rule is that it only *tells* you that a `from __future__ import annotations` is required, it doesn't have any way of applying the suggested fix.

Please add a suggestion or fix for this rule to automatically add the missing `from __future__ import annotations`.

**Code example:**
```py
# py 3.8 code base
def foo(l: list[int]): ...
```


---

_Comment by @charliermarsh on 2023-11-14 19:51_

One way to achieve this is via [`missing-required-import`](https://docs.astral.sh/ruff/rules/missing-required-import/) (`I002`) -- you can mark `from __future__ import annotations`, and it'll be added to all non-empty files (see: https://docs.astral.sh/ruff/settings/#isort-required-imports):

```toml
[tool.ruff.lint.isort]
required-imports = ["from __future__ import annotations"]
```

---

_Label `autofix` added by @charliermarsh on 2023-11-14 19:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-16 02:43_

---

_Comment by @charliermarsh on 2023-11-16 02:51_

I will fix this :)

---

_Referenced in [astral-sh/ruff#8711](../../astral-sh/ruff/pulls/8711.md) on 2023-11-16 03:01_

---

_Closed by @charliermarsh on 2023-11-16 03:08_

---

_Comment by @RunDevelopment on 2023-11-16 14:57_

Thank you @charliermarsh!

---

_Referenced in [chaiNNer-org/spandrel#9](../../chaiNNer-org/spandrel/pulls/9.md) on 2023-11-18 09:47_

---

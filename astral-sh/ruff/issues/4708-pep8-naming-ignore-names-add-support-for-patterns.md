```yaml
number: 4708
title: "`pep8-naming.ignore-names`: add support for patterns (wildcard)"
type: issue
state: closed
author: saippuakauppias
labels:
  - configuration
assignees: []
created_at: 2023-05-29T20:21:10Z
updated_at: 2023-05-29T23:01:21Z
url: https://github.com/astral-sh/ruff/issues/4708
synced_at: 2026-01-12T15:54:44Z
```

# `pep8-naming.ignore-names`: add support for patterns (wildcard)

---

_@saippuakauppias_

I think it would be useful to add pattern support to this option, so that you can set:
```
[tool.ruff.pep8-naming]
ignore-names = ["callMethod", "assert*"]
```

This is to avoid processing rule `invalid-function-name (N802)` on functions that are written in unittest and django-style:
- https://docs.python.org/3/library/unittest.html#unittest.TestCase.debug (see table)
- https://github.com/django/django/blob/4.1/django/test/testcases.py#L291 (see methods starts from `assert`)


---

_Label `configuration` added by @charliermarsh on 2023-05-29 20:52_

---

_Comment by @JonathanPlasse on 2023-05-29 22:52_

- Duplicate of #2787.

---

_Closed by @charliermarsh on 2023-05-29 23:01_

---

```yaml
number: 12758
title: "RUF031 with `parenthesize-tuple-in-subscript = true` should ignore type annotations"
type: issue
state: closed
author: ThiefMaster
labels:
  - bug
assignees: []
created_at: 2024-08-08T15:56:50Z
updated_at: 2024-08-09T19:31:29Z
url: https://github.com/astral-sh/ruff/issues/12758
synced_at: 2026-01-12T15:54:52Z
```

# RUF031 with `parenthesize-tuple-in-subscript = true` should ignore type annotations

---

_@ThiefMaster_

Feels like a bug to me that this applies to type annotations. In case of dict keys it's a very nice rule to make it clear that you have a tuple key, but for type annotations it feels pretty noisy/ugly...

```py
$ ruff version
ruff 0.5.7

$ cat test.py
bar = {}
bar[('hello', 'world')] = None
foo: tuple[str, str] | None = None

$ ruff check --isolated --select RUF031 --config 'lint.ruff.parenthesize-tuple-in-subscript = true' --preview --no-cache test.py
test.py:3:12: RUF031 [*] Use parentheses for tuples in subscripts.
  |
1 | bar = {}
2 | bar[('hello', 'world')] = None
3 | foo: tuple[str, str] | None = None
  |            ^^^^^^^^ RUF031
  |
  = help: Parenthesize the tuple.

Found 1 error.
[*] 1 fixable with the `--fix` option.
``

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-08 18:32_

---

_Label `bug` added by @charliermarsh on 2024-08-09 01:28_

---

_Closed by @AlexWaygood on 2024-08-09 19:31_

---

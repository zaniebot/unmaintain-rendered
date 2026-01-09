---
number: 16602
title: "[F811] Allow importing fixtures"
type: issue
state: open
author: alanhdu
labels:
  - rule
assignees: []
created_at: 2025-03-10T14:29:00Z
updated_at: 2025-03-10T18:31:25Z
url: https://github.com/astral-sh/ruff/issues/16602
synced_at: 2026-01-07T13:12:16-06:00
---

# [F811] Allow importing fixtures

---

_Issue opened by @alanhdu on 2025-03-10 14:29_

### Summary

We have some code where we define `pytest.fixture`s in a centralized place and then import them in our test suite. Something that looks like:

```python
from a import foo

def test_hello(foo) -> None:
    pass
```


Unfortunately, `ruff check` will then fail with:
```
test_b.py:1:15: F401 [*] `a.foo` imported but unused
  |
1 | from a import foo
  |               ^^^ F401
2 |
3 | def test_hello(foo) -> None:
  |
  = help: Remove unused import: `a.foo`

test_b.py:3:16: F811 Redefinition of unused `foo` from line 1
  |
1 | from a import foo
2 |
3 | def test_hello(foo) -> None:
  |                ^^^ F811
4 |     pass
  |
  = help: Remove definition: `foo`
```

Do you know what's going on with the F811 failure? I think the F401 failure is understandable (not sure we want to special-case logic that the fixture is automagically used by pytest, although that would certainly help us out), but I feel like the "redefinition of unused `foo`" error is weird. 

---

_Comment by @MichaReiser on 2025-03-10 18:31_

You can use to https://docs.astral.sh/ruff/settings/#lint_pyflakes_allowed-unused-imports to disable the `F401` warning. Unfortunately, this doesn't suppress F811 which is mainly what you're looking for. Although I'm not sure if ignoring the imports lited in `allowed-unused-imports` is in the spirit of `F811` outside of pytests. Have you considered disabling `F811` in tests?

---

_Label `rule` added by @MichaReiser on 2025-03-10 18:31_

---

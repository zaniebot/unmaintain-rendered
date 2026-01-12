```yaml
number: 4932
title: "`if-else-block-instead-of-if-exp` autofix predates the desired `if-else-block-instead-of-dict-get` autofix"
type: issue
state: closed
author: tjkuson
labels: []
assignees: []
created_at: 2023-06-07T16:08:06Z
updated_at: 2023-06-07T22:33:41Z
url: https://github.com/astral-sh/ruff/issues/4932
synced_at: 2026-01-12T15:54:45Z
```

# `if-else-block-instead-of-if-exp` autofix predates the desired `if-else-block-instead-of-dict-get` autofix

---

_@tjkuson_

## Expected behaviour

`ruff check --fix --select SIM .` should autofix

```python
obj: dict = ...
key = ...

if key in obj:
    value = obj[key]
else:
    value = "Not found"
```

to this:

```python
obj: dict = ...
key = ...

value = obj.get(key, "Not found")
```

## Actual behaviour

`ruff check --fix --select SIM .` autofixes the above code to

```python
obj: dict = ...
key = ...

value = obj[key] if key in obj else "Not found"
```

This happens because `if-else-block-instead-of-if-exp` (`SIM108`) autofixes before `if-else-block-instead-of-dict-get` (`SIM401`), and `SIM401` does not flag violations in ternary operations.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-07 22:24_

---

_Closed by @charliermarsh on 2023-06-07 22:33_

---

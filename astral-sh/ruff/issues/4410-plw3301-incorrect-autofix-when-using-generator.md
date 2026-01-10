```yaml
number: 4410
title: "PLW3301: incorrect autofix when using generator expression"
type: issue
state: closed
author: oefe
labels:
  - bug
assignees: []
created_at: 2023-05-13T12:07:37Z
updated_at: 2023-05-13T15:17:14Z
url: https://github.com/astral-sh/ruff/issues/4410
synced_at: 2026-01-10T11:09:47Z
```

# PLW3301: incorrect autofix when using generator expression

---

_Issue opened by @oefe on 2023-05-13 12:07_

The autofix for `PLW3301` generates incorrect code if the nested call is using a generator expression.

## Example

PLW3301.py:
```python
x = max(1, max(i for i in range(10)))
```

## Observed 
```
ruff --isolated --diff --select PLW3301  PLW3301.py
--- PLW3301.py
+++ PLW3301.py
@@ -1 +1 @@
-x = max(1, max(i for i in range(10)))
+x = max(1, (i for i in range(10)))

Would fix 1 error.
```

## Issue

This is incorrect. `max` [takes either an iterable, or a variable number of arguments, but not both](https://docs.python.org/3/library/functions.html#max):
```
python -c "max(1, (i for i in range(10)))"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
TypeError: '>' not supported between instances of 'generator' and 'int'
```                                        

## Expected

The correct fix would be:
```
max(1, *(i for i in range(10)))
```

## Ruff Version

```
ruff --version
ruff 0.0.267
```



---

_Label `bug` added by @charliermarsh on 2023-05-13 12:40_

---

_Comment by @JonathanPlasse on 2023-05-13 12:44_

I can work on it.

---

_Assigned to @JonathanPlasse by @MichaReiser on 2023-05-13 12:50_

---

_Comment by @charliermarsh on 2023-05-13 12:52_

We may just want to avoid including `max` calls with a single argument altogether, since max seems to assume that the single argument is iterable.

---

_Closed by @charliermarsh on 2023-05-13 15:17_

---

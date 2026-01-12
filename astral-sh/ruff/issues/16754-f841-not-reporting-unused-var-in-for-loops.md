```yaml
number: 16754
title: F841 not reporting unused var in for loops
type: issue
state: closed
author: sk-
labels:
  - question
assignees: []
created_at: 2025-03-14T16:09:25Z
updated_at: 2025-03-14T17:15:30Z
url: https://github.com/astral-sh/ruff/issues/16754
synced_at: 2026-01-12T15:54:55Z
```

# F841 not reporting unused var in for loops

---

_@sk-_

### Summary

F841 is not reporting unused loop vars as unused. This can be seen in the following code ([playground](https://play.ruff.rs/f5ecf49d-df91-4033-adaf-a7ad42c39f07))

```python
def func():
    for x in []:
        pass

    data = {}
    for key1, val1 in data.items():
        pass

    for key2, val2 in data.items():
        print(key2)

    for ke3, val3 in data.items():
        print(val3)
```
In the code above, if the variables were reported as unused, the programmer, could be able to realize that better constructs exist for their use case, replacing the last two loops with

```python
     for key2 in data:
        print(key2)

    for val3 in data.values():
        print(val3)
```

May be related to #16753. 

### Version

v0.11.0 (playground)

---

_Comment by @ntBre on 2025-03-14 16:50_

I believe this use case is covered by [`unused-loop-control-variable`](https://docs.astral.sh/ruff/rules/unused-loop-control-variable/#unused-loop-control-variable-b007) (`B007`) instead.

---

_Label `question` added by @ntBre on 2025-03-14 16:50_

---

_Closed by @sk- on 2025-03-14 17:15_

---

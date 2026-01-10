```yaml
number: 389
title: "error[unresolved-reference] nested functions false positive"
type: issue
state: closed
author: jc-louis
labels:
  - control flow
assignees: []
created_at: 2025-05-14T16:57:28Z
updated_at: 2025-05-14T17:46:42Z
url: https://github.com/astral-sh/ty/issues/389
synced_at: 2026-01-10T02:34:09Z
```

# error[unresolved-reference] nested functions false positive

---

_Issue opened by @jc-louis on 2025-05-14 16:57_

### Summary

Hello,
I'm having false positive with `error[unresolved-reference]` in a very specific case of nested functions.

Minimal example:
```py
def bug_inner() -> str:
    if True: # no error without this if
        inner = "x"

        def _inner_fct() -> str:
            if "x" == inner:
                return "x"

        return _inner_fct()
```

The error:
```
error[unresolved-reference]: Name `inner` used when not defined
 --> scratch_1425.py:6:23
  |
5 |         def _inner_fct() -> str:
6 |             if "x" == inner:
  |                       ^^^^^
7 |                 return "x"
  |
```

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Label `control flow` added by @AlexWaygood on 2025-05-14 17:41_

---

_Comment by @carljm on 2025-05-14 17:46_

Thanks for the report! This is a useful concise example case for https://github.com/astral-sh/ty/issues/210 -- I think I will close it as a duplicate, but it's very useful, thank you.

---

_Closed by @carljm on 2025-05-14 17:46_

---

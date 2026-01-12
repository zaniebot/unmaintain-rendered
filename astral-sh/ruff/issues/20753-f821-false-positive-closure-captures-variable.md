```yaml
number: 20753
title: "F821 false positive: closure captures variable before `del` statement"
type: issue
state: closed
author: lamteteeow
labels: []
assignees: []
created_at: 2025-10-07T20:36:05Z
updated_at: 2025-10-08T13:01:37Z
url: https://github.com/astral-sh/ruff/issues/20753
synced_at: 2026-01-12T15:54:57Z
```

# F821 false positive: closure captures variable before `del` statement

---

_@lamteteeow_

### Summary

## Description

Ruff incorrectly reports F821 "Undefined name" when a variable is captured by a nested function (closure) before being deleted with `del` in the outer scope.
This has been occurring since ruff 0.13.3 first time I encountered it.

## Minimal Reproduction

```python
def main(A: int) -> int:
    A = A + 1
    def func_inner(B: int) -> int:
        return A + B  # F821: Undefined name `A` <- FALSE POSITIVE
    C = func_inner(3)
    del A
    return C

if __name__ == "__main__":
    result = main(10)
    print(result)  # Outputs: 14
```

### Version

ruff 0.14.0 (2025-10-07)

---

_Comment by @ntBre on 2025-10-07 20:51_

Thanks for the report! I think this is a duplicate of #9858. It's a tricky bug to fix, unfortunately.

---

_Closed by @ntBre on 2025-10-07 20:51_

---

_Comment by @lamteteeow on 2025-10-07 21:23_

Thanks for quick response to this. It does not bother me too much. I saw the other issue but I was not sure they rooted from the same problem so deliberately posted another one. Sorry for that.

---

_Comment by @ntBre on 2025-10-08 13:01_

No problem at all, it's good to double check! Thanks again :)

---

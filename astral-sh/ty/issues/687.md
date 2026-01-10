```yaml
number: 687
title: Exhaustive enum checks do not work
type: issue
state: closed
author: scosman
labels: []
assignees: []
created_at: 2025-06-20T16:15:48Z
updated_at: 2025-06-20T16:25:03Z
url: https://github.com/astral-sh/ty/issues/687
synced_at: 2026-01-10T02:08:20Z
```

# Exhaustive enum checks do not work

---

_Issue opened by @scosman on 2025-06-20 16:15_

### Summary

I use assert_never in pyright to catch missing enum cases, but they don't work in ty.

Example below (SomeEnum only has the 2 possible values):
```
def matching(x: SomeEnum):
    match x:
        case SomeEnum.first:
            pass
        case SomeEnum.second:
            pass
        case _:
            # not detected as unreachable
            assert_never(x)  # ty error `Never`, found `SomeEnum & @Todo` even though this is unreachable
```

Expected: no ty error when all enum bases are covered in the match, but a ty error when any are missing.

Similar to https://github.com/astral-sh/ty/issues/99 but different.

### Version

0.0.1a11

---

_Comment by @AlexWaygood on 2025-06-20 16:25_

Thanks! This is a duplicate of #99 in combination with https://github.com/astral-sh/ty/issues/183. Fixing those two issues should resolve this.

---

_Closed by @AlexWaygood on 2025-06-20 16:25_

---

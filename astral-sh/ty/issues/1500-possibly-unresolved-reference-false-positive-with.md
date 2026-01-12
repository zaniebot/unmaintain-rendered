```yaml
number: 1500
title: "`possibly-unresolved-reference` false positive with walrus operator"
type: issue
state: closed
author: aytey
labels: []
assignees: []
created_at: 2025-11-07T15:23:40Z
updated_at: 2025-11-07T16:07:59Z
url: https://github.com/astral-sh/ty/issues/1500
synced_at: 2026-01-12T15:54:25Z
```

# `possibly-unresolved-reference` false positive with walrus operator

---

_@aytey_

### Summary

I don't think that this is a duplicate of https://github.com/astral-sh/ty/issues/162 as there's no list comprehension here.

For the following code:

```py
def moo():
    return 10


if __name__ == "__main__":

    if moo() and (x := moo()):
        print(x)
```

`ty check --error possibly-unresolved-reference moo.py` gives:

```
error[possibly-unresolved-reference]: Name `x` used when possibly not defined
 --> moo.py:8:15
  |
7 |     if moo() and (x := moo()):
8 |         print(x)
  |               ^
  |
info: rule `possibly-unresolved-reference` was selected on the command line
```

The reason I am calling this a false positive is that it is not "possibly not defined" -- if you get to Line 8, the `x` will definitely be defined.

### Version

ty 0.0.1-alpha.25

---

_Comment by @carljm on 2025-11-07 16:07_

Thanks for the report! This is https://github.com/astral-sh/ty/issues/626

---

_Closed by @carljm on 2025-11-07 16:08_

---

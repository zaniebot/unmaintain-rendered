```yaml
number: 2216
title: "valid `RET507 Unnecessary `elif` after `continue` statement` ?"
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-26T19:16:21Z
updated_at: 2023-01-26T21:17:09Z
url: https://github.com/astral-sh/ruff/issues/2216
synced_at: 2026-01-10T11:09:45Z
```

# valid `RET507 Unnecessary `elif` after `continue` statement` ?

---

_Issue opened by @spaceone on 2023-01-26 19:16_

I get `RET507` Unnecessary `elif` after `continue` statement for:

```
def something(stdout):
    for line in stdout:
        if line.startswith('foo'):
            pass
        elif not line.strip():
            continue
        elif line.startswith('bar'):
            pass
        elif line.startswith('baz'):
            pass
```

I consider this code to be okay, that we should not transform the `elif` to a new `if`:

Btw. if the code is not inside of a function the `RET507` is not detected.

---

_Comment by @spaceone on 2023-01-26 19:23_

same for `RET508` where `break` instead of `continue` is used.

---

_Comment by @charliermarsh on 2023-01-26 21:17_

This is consistent with the upstream rule. I agree it's somewhat confusing, but I believe it's "correct" given the rule.

---

_Closed by @charliermarsh on 2023-01-26 21:17_

---

```yaml
number: 6821
title: "Ignoring commented-out code with noqa: ERA001 may still trigger RUF100"
type: issue
state: closed
author: yigor
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-08-23T17:19:00Z
updated_at: 2023-08-25T19:26:53Z
url: https://github.com/astral-sh/ruff/issues/6821
synced_at: 2026-01-10T11:09:49Z
```

# Ignoring commented-out code with noqa: ERA001 may still trigger RUF100

---

_Issue opened by @yigor on 2023-08-23 17:19_

Looks like #1148 wasn't completely fixed:

```
# commented.py

dictionary = {
    # "key1": 123,  # noqa: ERA001
    # "key2": 456,
}
```
executing ruff against this file results in the following:
```
$ ruff commented.py                                                                                                                                                              master
commented.py:2:21: RUF100 [*] Unused `noqa` directive (unused: `ERA001`)
commented.py:3:5: ERA001 [*] Found commented-out code
Found 2 errors.
[*] 2 potentially fixable with the --fix option.

$ ruff --version
ruff 0.0.285
```



---

_Comment by @zanieb on 2023-08-23 17:32_

Thanks for raising! I've reproduced this in #6822 although I'm not sure what the fix is yet. I'm not sure how https://github.com/astral-sh/ruff/pull/1157 was intended to address this.

---

_Label `bug` added by @zanieb on 2023-08-23 17:33_

---

_Label `accepted` added by @zanieb on 2023-08-23 17:33_

---

_Closed by @zanieb on 2023-08-25 16:02_

---

_Comment by @yigor on 2023-08-25 19:26_

Awesome, thank you!

---

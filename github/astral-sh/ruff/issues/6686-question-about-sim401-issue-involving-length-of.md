---
number: 6686
title: Question about SIM401 issue involving length of strings
type: issue
state: closed
author: nth10sd
labels: []
assignees: []
created_at: 2023-08-18T20:30:35Z
updated_at: 2023-08-18T21:11:20Z
url: https://github.com/astral-sh/ruff/issues/6686
synced_at: 2026-01-07T13:12:15-06:00
---

# Question about SIM401 issue involving length of strings

---

_Issue opened by @nth10sd on 2023-08-18 20:30_

This testcase does not show `SIM401`:

```
import os

a = {}

if "A123456789012" in os.environ:
    a["A123456789012"] = os.environ["A123456789012"]
else:
    a["A123456789012"] = "123456789012345678901234567890123"
```

However, shortening the length of the `A123456789012` key by deleting the last `2` or shortening `123456789012345678901234567890123` by deleting the last `3` causes the issue to be detected by `ruff`, for example:

```
test.py:5:1: SIM401 [*] Use `a["A123456789012"] = os.environ.get("A123456789012", "12345678901234567890123456789012")` instead of an `if` block
```

May I know if this is intended?

```
$ ruff --version
ruff 0.0.285
```

---

_Comment by @charliermarsh on 2023-08-18 20:48_

I believe we avoid flagging this if the fix would cause the code to exceed the line length?

---

_Comment by @charliermarsh on 2023-08-18 20:48_

That’s my guess, I’d have to look it up later though if it doesn’t explain it.

---

_Comment by @charliermarsh on 2023-08-18 21:11_

Yes that's the explanation. We may change this behavior at some point in the future once we have our own formatter.

---

_Closed by @charliermarsh on 2023-08-18 21:11_

---

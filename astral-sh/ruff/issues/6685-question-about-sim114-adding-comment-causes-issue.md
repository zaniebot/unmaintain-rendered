```yaml
number: 6685
title: Question about SIM114, adding comment causes issue not to be detected
type: issue
state: closed
author: nth10sd
labels: []
assignees: []
created_at: 2023-08-18T20:11:40Z
updated_at: 2023-08-18T20:25:05Z
url: https://github.com/astral-sh/ruff/issues/6685
synced_at: 2026-01-12T15:54:46Z
```

# Question about SIM114, adding comment causes issue not to be detected

---

_@nth10sd_

This testcase does not trigger SIM114:

```
x = "x"
y = "y"

if x == "x":
    pass
elif x == "y":
    # Comment
    pass
elif y == "y":
    pass
```

However, removing the comment causes `ruff` to detect SIM114 as expected:

```
x = "x"
y = "y"

if x == "x":
    pass
elif x == "y":
    pass
elif y == "y":
    pass
```

```
test.py:4:1: SIM114 Combine `if` branches using logical `or` operator
test.py:6:1: SIM114 Combine `if` branches using logical `or` operator
```

May I know if this is expected?

```
$ ruff --version
ruff 0.0.285
```

---

_Comment by @charliermarsh on 2023-08-18 20:17_

This is intentional. If the branches have their own comments, it's typically seen as a sign that it's intentional and should not be flagged.

---

_Comment by @nth10sd on 2023-08-18 20:25_

Got it, thanks!

---

_Closed by @nth10sd on 2023-08-18 20:25_

---

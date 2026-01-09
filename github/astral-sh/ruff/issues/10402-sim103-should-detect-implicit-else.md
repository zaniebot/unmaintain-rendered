---
number: 10402
title: "`SIM103` should detect implicit `else`"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - rule
assignees: []
created_at: 2024-03-14T02:35:26Z
updated_at: 2024-03-18T00:15:29Z
url: https://github.com/astral-sh/ruff/issues/10402
synced_at: 2026-01-07T13:12:15-06:00
---

# `SIM103` should detect implicit `else`

---

_Issue opened by @charliermarsh on 2024-03-14 02:35_

See: https://github.com/sbdchd/flake8-pie#pie801-prefer-simple-return.

In that example:

```python
# error
def main():
    if foo > 5:
        return True
    return False

# error
def main():
    if foo > 5:
        return True
    else:
        return False

# ok
def main():
    return foo > 5
```

We catch the second case, but not the first.

---

_Label `good first issue` added by @charliermarsh on 2024-03-14 02:35_

---

_Label `rule` added by @charliermarsh on 2024-03-14 02:35_

---

_Referenced in [Zac-HD/shed#22](../../Zac-HD/shed/issues/22.md) on 2024-03-14 02:44_

---

_Comment by @ottaviohartman on 2024-03-14 20:42_

@charliermarsh I'd like to give this a shot.

---

_Assigned to @ottaviohartman by @charliermarsh on 2024-03-14 20:45_

---

_Comment by @charliermarsh on 2024-03-14 20:45_

Great!

---

_Referenced in [astral-sh/ruff#10414](../../astral-sh/ruff/pulls/10414.md) on 2024-03-15 00:11_

---

_Closed by @charliermarsh on 2024-03-18 00:15_

---

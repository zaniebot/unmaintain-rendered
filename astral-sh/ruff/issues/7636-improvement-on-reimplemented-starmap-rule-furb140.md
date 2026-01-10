```yaml
number: 7636
title: Improvement on reimplemented-starmap rule (FURB140)
type: issue
state: closed
author: alexprengere
labels:
  - good first issue
  - rule
  - accepted
assignees: []
created_at: 2023-09-25T08:00:51Z
updated_at: 2023-10-03T01:38:07Z
url: https://github.com/astral-sh/ruff/issues/7636
synced_at: 2026-01-10T11:09:49Z
```

# Improvement on reimplemented-starmap rule (FURB140)

---

_Issue opened by @alexprengere on 2023-09-25 08:00_

I tested Ruff 0.0.291 which implements FURB140. It works great! I have a suggestion to cover more use cases in the same spirit:

```python
def average(*args):
    return sum(args) / len(args)

# Correctly reported
print(sum(average(a, b) for a, b in [(85, 60), (100, 80)]))

# Currently not reported
print(sum(average(*t) for t in [(85, 60), (100, 80)]))
```

---

_Label `rule` added by @charliermarsh on 2023-09-25 13:05_

---

_Label `accepted` added by @charliermarsh on 2023-09-25 13:05_

---

_Label `good first issue` added by @charliermarsh on 2023-09-25 15:42_

---

_Comment by @charliermarsh on 2023-09-25 15:42_

Makes sense! 

---

_Closed by @charliermarsh on 2023-10-03 01:38_

---

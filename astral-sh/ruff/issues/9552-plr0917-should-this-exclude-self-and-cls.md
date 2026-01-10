```yaml
number: 9552
title: "PLR0917: should this exclude `self` and `cls`?"
type: issue
state: closed
author: tmke8
labels:
  - rule
assignees: []
created_at: 2024-01-16T15:28:33Z
updated_at: 2024-01-18T03:17:47Z
url: https://github.com/astral-sh/ruff/issues/9552
synced_at: 2026-01-10T11:09:51Z
```

# PLR0917: should this exclude `self` and `cls`?

---

_Issue opened by @tmke8 on 2024-01-16 15:28_

[PLR0917](https://docs.astral.sh/ruff/rules/too-many-positional/) counts the number of non-keyword-only arguments and checks whether the number is above `max-positional-args`. This count includes `self` and `cls` in methods and class-methods, but I feel like those shouldn't count because they are passed in implicitly.

I'd be willing to make the code change if this is acceptable.

---

_Comment by @charliermarsh on 2024-01-16 19:13_

Yeah, I think it probably should exclude those.

---

_Label `rule` added by @charliermarsh on 2024-01-16 19:13_

---

_Closed by @charliermarsh on 2024-01-18 03:17_

---

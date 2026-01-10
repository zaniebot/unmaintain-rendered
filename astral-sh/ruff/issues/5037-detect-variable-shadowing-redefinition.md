```yaml
number: 5037
title: Detect variable shadowing / redefinition
type: issue
state: open
author: stefanv
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-12T20:42:09Z
updated_at: 2024-06-27T13:51:33Z
url: https://github.com/astral-sh/ruff/issues/5037
synced_at: 2026-01-10T11:09:47Z
```

# Detect variable shadowing / redefinition

---

_Issue opened by @stefanv on 2023-06-12 20:42_

```
x = 3

x = 4
y = x + 3
```

It would be great to have a rule that detects when a variable is assigned to a second time without being used. I'm not sure how feasible this would be in general, given how convoluted control flow is, but there are at least a few straightforward cases such as the above.

---

_Comment by @charliermarsh on 2023-06-12 20:51_

I think we follow Pyflakes behavior here, which is to detect this in some cases -- e.g., if the thing being shadowed is an import:

```py
def f():
    import x

    x = 4
    y = x + 3
```

---

_Label `question` added by @charliermarsh on 2023-06-13 17:13_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:11_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:11_

---

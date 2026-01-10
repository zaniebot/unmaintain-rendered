```yaml
number: 8964
title: "Rule suggestion: lint against the use of `in` with compound comparisons"
type: issue
state: open
author: tjkuson
labels:
  - rule
assignees: []
created_at: 2023-12-02T13:39:02Z
updated_at: 2024-01-25T20:44:13Z
url: https://github.com/astral-sh/ruff/issues/8964
synced_at: 2026-01-10T11:09:51Z
```

# Rule suggestion: lint against the use of `in` with compound comparisons

---

_Issue opened by @tjkuson on 2023-12-02 13:39_

The use of `in` with compound comparisons often leads to unintended/surprising behaviour.

```python
False == False in [False]  # True
```

A solution would be to use parentheses.

```python
(False == False) in [False]  # False
False == (False in [False])  # False
```

Similar to #8721, but the AST produced by `False or False and False` is quite different, so might be a different rule.

---

_Label `rule` added by @charliermarsh on 2023-12-02 13:53_

---

_Comment by @JelleZijlstra on 2023-12-02 16:21_

Agree this would be useful to lint against. There are a few more variations of compound `Compare` that probably don't make sense either: `a < b is c`, `a is b is not c`.

Arguably we should lint against all compound Compares *except* the following:
* Both compares are `is`: `a is b is c`
* Both compares are `==`: `a == b ==c`
* Both compares are one of `<`, `<=`: `a <= b < c` (probably the most common use  of compound Compare)
* Both compares are one of `>`, `>=`: `a >= b > c`

Maybe there are other uses of compound Compare that also make sense and that I didn't think of, though.

---

_Comment by @tjkuson on 2024-01-25 20:38_

I'll submit a PR implementing this rule this weekend, assuming it's been accepted @charliermarsh 

---

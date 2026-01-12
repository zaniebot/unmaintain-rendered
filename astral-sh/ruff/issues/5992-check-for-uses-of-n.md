```yaml
number: 5992
title: "Check for uses of `--n`"
type: issue
state: closed
author: tjkuson
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-07-22T23:25:12Z
updated_at: 2023-07-23T01:40:51Z
url: https://github.com/astral-sh/ruff/issues/5992
synced_at: 2026-01-12T15:54:45Z
```

# Check for uses of `--n`

---

_@tjkuson_

`unary-prefix-increment` (`B002`) warns users about

```python
++n
```

but not 

```python
--n
```

It would be good if either the rule were changed to check for both, or if a new rule were created to check for unary prefix decrement operators. The rationale is the same; users coming from other languages which support `++` and `--` unary prefix operators might not realise that these 'operators' in Python don't do anything, causing subtle bugs.

---

_Label `accepted` added by @charliermarsh on 2023-07-23 00:16_

---

_Comment by @charliermarsh on 2023-07-23 00:16_

Fine with me to add to the existing rule.

---

_Label `rule` added by @charliermarsh on 2023-07-23 00:16_

---

_Closed by @charliermarsh on 2023-07-23 01:40_

---

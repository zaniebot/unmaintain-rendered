```yaml
number: 4934
title: "Remove falsy dict.get() fallback in `if` statements"
type: issue
state: closed
author: janosh
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-06-07T18:08:12Z
updated_at: 2024-12-31T10:40:53Z
url: https://github.com/astral-sh/ruff/issues/4934
synced_at: 2026-01-12T15:54:45Z
```

# Remove falsy dict.get() fallback in `if` statements

---

_@janosh_

```py
# bad
if dct.get(key, False):
if dct.get(key, ""):
if dct.get(key, []):
if dct.get(key, {}):

# good
if dct.get(key):

# bad
if dct.get(key, set()) and other_cond:
if dct.get(key, 0) or other_cond:

# good
if dct.get(key) and other_cond:
if dct.get(key) or other_cond:
```

---

_Label `rule` added by @charliermarsh on 2023-06-07 19:01_

---

_Comment by @evanrittenhouse on 2023-06-08 00:52_

@charliermarsh Would this just be a `RUF` rule?

---

_Comment by @janosh on 2023-06-08 04:03_

Actually if statement is not needed. Any boolean evaluation.

---

_Comment by @dhruvmanila on 2023-06-08 05:19_

```python
# bad
if dct.get(key) and other_cond:
if dct.get(key) or other_cond:
```

Can you expand on why this is bad? I don't think it is as the `other_cond` will be evaluated if the `.get` is truthy in the `and` case and falsy in the `or` case.

This would also require type inference without which we're limited to certain cases.

---

_Comment by @evanrittenhouse on 2023-06-08 05:27_

@dhruvmanila just a heads up, I don't think the boolean expression is necessary (see the comment above yours :) )

---

_Comment by @janosh on 2023-06-08 06:08_

> Can you expand on why this is bad?

Sorry, that was an oversight. I meant `# good`. Fixed above.

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:27_

---

_Closed by @MichaReiser on 2024-12-31 10:40_

---

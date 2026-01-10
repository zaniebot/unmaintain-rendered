---
number: 13242
title: False positive D402 when mentioning a different function that has the function name as suffix
type: issue
state: closed
author: Daraan
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-09-04T12:55:46Z
updated_at: 2024-09-18T04:01:39Z
url: https://github.com/astral-sh/ruff/issues/13242
synced_at: 2026-01-10T01:22:53Z
---

# False positive D402 when mentioning a different function that has the function name as suffix

---

_Issue opened by @Daraan on 2024-09-04 12:55_

I stumbled on the following false positive cases where the mention of `prefix_foo` flags a D402 on a function called `foo`

```python
def foo(self):
    """ Use prefix_foo() """
    # ^^^^^^^^^^^^^^^^^^^ D402
```

---

Version: `ruff 0.6.3`

Command: `ruff check . --select D402 --isolated`

Related #3769; where the same function is mentioned explicitly without a prefix.


---

_Label `bug` added by @dhruvmanila on 2024-09-05 06:33_

---

_Label `docstring` added by @dhruvmanila on 2024-09-05 06:33_

---

_Comment by @dhruvmanila on 2024-09-05 06:34_

I think we can solve this by using whole words instead, so look for `foo` and not any words that ends with `foo`. Curious to know what the ecosystem changes would look like.

---

_Renamed from "False positive D402 when mentioning a different function that has the fuction as suffix" to "False positive D402 when mentioning a different function that has the function name as suffix" by @Daraan on 2024-09-05 08:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-18 03:47_

---

_Referenced in [astral-sh/ruff#13388](../../astral-sh/ruff/pulls/13388.md) on 2024-09-18 03:50_

---

_Closed by @charliermarsh on 2024-09-18 04:01_

---

```yaml
number: 7205
title: "Rule suggestion: Prevent walrus assignments in assert statements"
type: issue
state: closed
author: awaelchli
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-09-06T17:08:19Z
updated_at: 2023-10-09T19:35:13Z
url: https://github.com/astral-sh/ruff/issues/7205
synced_at: 2026-01-10T11:09:49Z
```

# Rule suggestion: Prevent walrus assignments in assert statements

---

_Issue opened by @awaelchli on 2023-09-06 17:08_

If you want to make your software package run with the `python -O` optimization, you might want to ensure that the code does not contain a walrus assignment in `assert` statements:

```py
y = 0
assert (x := y) is not None
x = x + 1
```

This code example runs fine normally, but if run with `python -O` the assertion gets removed and the program raises a `UnboundLocalError` because x is no longer bound. 

As a developer, I might want to configure my linter in such a way that walrus assignments are not allowed in assertion statements. This way, I can enforce that the code will be compatible running with `python -O`. 


---

_Renamed from "Rule suggestion: Prevent usage of walrus assignments in assert statements" to "Rule suggestion: Prevent walrus assignments in assert statements" by @awaelchli on 2023-09-06 17:10_

---

_Label `rule` added by @zanieb on 2023-09-06 17:12_

---

_Label `needs-decision` added by @zanieb on 2023-09-06 17:12_

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-22 04:08_

---

_Label `accepted` added by @charliermarsh on 2023-09-22 04:08_

---

_Comment by @charliermarsh on 2023-09-22 04:08_

Seems reasonable to me. We can add it as a `RUF` rule.

---

_Closed by @charliermarsh on 2023-10-09 19:35_

---

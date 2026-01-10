```yaml
number: 7288
title: SIM401 should catch ternary operations
type: issue
state: closed
author: jerr0328
labels:
  - good first issue
  - help wanted
  - accepted
assignees: []
created_at: 2023-09-12T08:46:38Z
updated_at: 2023-10-20T20:47:02Z
url: https://github.com/astral-sh/ruff/issues/7288
synced_at: 2026-01-10T11:09:49Z
```

# SIM401 should catch ternary operations

---

_Issue opened by @jerr0328 on 2023-09-12 08:46_

In #4932, there was a bug because if-else blocks were rewritten as ternary operations which SIM401 wouldn't be able to fix. The bug was fixed but there's still the fact that you can have code like

```python
obj: dict = ...
key = ...

value = obj[key] if key in obj else "Not found"
```

which will not be corrected with `ruff check --fix --select SIM .`

Expected behavior:
```python
obj: dict = ...
key = ...

value = obj.get(key, "Not found")
``` 

Checked with ruff `v0.0.287`



---

_Label `rule` added by @charliermarsh on 2023-09-12 11:54_

---

_Label `accepted` added by @charliermarsh on 2023-09-12 11:54_

---

_Comment by @charliermarsh on 2023-09-12 11:54_

Makes sense to me.

---

_Label `good first issue` added by @charliermarsh on 2023-09-12 13:12_

---

_Label `help wanted` added by @charliermarsh on 2023-09-12 13:12_

---

_Label `rule` removed by @charliermarsh on 2023-09-12 13:12_

---

_Comment by @Flowrey on 2023-09-14 17:39_

Hey, I would like to give it a try can I ?

---

_Comment by @zanieb on 2023-09-14 18:04_

@Flowrey feel free!

---

_Assigned to @Flowrey by @zanieb on 2023-09-14 18:04_

---

_Comment by @Flowrey on 2023-09-14 20:11_

I'm a bit puzzled, ternary operations are expressions, `Expr::IfExp`, but `if-else-block-instead-of-dict-get` rule is defined for statement and expect an `StmtIf` as argument, so should i define a new rule in this case like `if-expr-instead-of-dict-get` ?

---

_Comment by @charliermarsh on 2023-09-14 20:15_

I think it can be the same _rule_, but it will need to be a new function that takes an `Expr::IfExp`, and is called from `crates/ruff/src/checkers/ast/analyze/expression.rs`. We may also need to rename the rule to reflect the two cases.

---

_Closed by @charliermarsh on 2023-10-20 20:47_

---

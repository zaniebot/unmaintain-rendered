```yaml
number: 4242
title: "`RET504` not triggered with arbitrary calls"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2023-05-05T10:55:51Z
updated_at: 2023-06-10T04:05:04Z
url: https://github.com/astral-sh/ruff/issues/4242
synced_at: 2026-01-12T15:54:44Z
```

# `RET504` not triggered with arbitrary calls

---

_@dhruvmanila_

What about the following code:

```python
def func() -> int:
    match True:
        case True:
            x = 5
            return x
        case _:
            raise ValueError("Error")
```

I don't get `RET504` here as well. Is that the expected behavior?

_Originally posted by @Apakottur in https://github.com/charliermarsh/ruff/issues/4240#issuecomment-1536067821_
            

---

_Comment by @dhruvmanila on 2023-05-05 10:57_

After giving a quick look, it's occurring as the `Visitor` is not going through the `match` statement.

---

_Label `bug` added by @charliermarsh on 2023-05-05 18:02_

---

_Label `good first issue` added by @charliermarsh on 2023-05-05 18:03_

---

_Comment by @dhruvmanila on 2023-05-06 14:12_

Scratch my initial analysis, visitor pattern means that it'll travel to every node _unless_ explicitly avoided. We're going through the match statement, but the problem is two fold:
1. `match` is a mutually exclusive statement. This means that a binding in one case can't be referenced in another case. Visitor is not scope aware.
2. The comment below means that we're considering `x` to be referenced inside the `ValueError` call which is what is creating this false negative.

https://github.com/charliermarsh/ruff/blob/bb2cbf1f2549c7472f0caf913ea4896b2aae0dc4/crates/ruff/src/rules/flake8_return/visitor.rs#L172-L183

Solving (2) seems to be a bit difficult but I think for the short term we can avoid increasing the reference count for builtin calls.

For the long term, the scope will have to be implemented which can be used to increase the reference count of variables which the function has access to, either directly through the arguments or from an outer scope. We could even go further and traverse the function body to check if it's actually being referenced but there might be a challenge in differentiating between referencing the same variable name defined inside the function vs actually referencing it from outside the scope.

These are just some of my thoughts after debugging this issue. I think this might not be a _good first issue_.

---

_Renamed from "`RET504` not triggered for `match` statement" to "`RET504` not triggered with arbitrary calls" by @dhruvmanila on 2023-05-06 14:13_

---

_Label `good first issue` removed by @charliermarsh on 2023-05-06 15:32_

---

_Comment by @charliermarsh on 2023-05-08 22:40_

I am tempted to change this rule such that it only detects:

```py
x = 5
return x
```

That is: such that it only detects "assignment followed by return". 

---

_Closed by @charliermarsh on 2023-06-10 04:05_

---

_Closed by @charliermarsh on 2023-06-10 04:05_

---

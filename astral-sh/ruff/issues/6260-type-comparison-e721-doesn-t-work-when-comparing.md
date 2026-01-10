```yaml
number: 6260
title: "type-comparison (E721) doesn't work when comparing against an actual type"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - rule
assignees: []
created_at: 2023-08-02T00:38:14Z
updated_at: 2023-08-09T05:06:15Z
url: https://github.com/astral-sh/ruff/issues/6260
synced_at: 2026-01-10T11:09:48Z
```

# type-comparison (E721) doesn't work when comparing against an actual type

---

_Issue opened by @DetachHead on 2023-08-02 00:38_

```py
foo = 1
if type(foo) is int: # no error
    ...
if type(foo) is type(1): # error
    ...
```
the pylint rule `unidiomatic-typecheck` catches both cases

---

_Label `bug` added by @zanieb on 2023-08-02 03:32_

---

_Label `rule` added by @zanieb on 2023-08-02 03:32_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-03 23:55_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-08-04 02:06_

---

_Closed by @charliermarsh on 2023-08-04 02:25_

---

_Comment by @KotlinIsland on 2023-08-04 03:12_

@charliermarsh Could you please clarify if the change here applies to only builtin types? And if so why, because in pylint it applies to any type, not just builtins:

```py
class A: ...
a = A()
type(a) == A  # C0123: Use isinstance() rather than type() for a typecheck. (unidiomatic-typecheck)
```

---

_Comment by @charliermarsh on 2023-08-04 03:25_

I believe that allowing arbitrary expressions on the right-hand side will lead to false positives for legitimate usages, as in: https://github.com/PyCQA/pycodestyle/issues/882#issuecomment-588480243.

---

_Comment by @KotlinIsland on 2023-08-04 03:44_

Oh, is this because Ruff isn't able to tell if the expression on the rhs is a `type` or not?

---

_Comment by @charliermarsh on 2023-08-04 03:49_

Yes that's correct. E.g., this is contrived but seems reasonable?

```python
type_of_b = type(b)
type(a) is type_of_b
```

Pylint flags this under `unidiomatic-typecheck`, but it seems fine to me.

---

_Comment by @KotlinIsland on 2023-08-04 04:47_

> Yes that's correct.

If Ruff can't determine if the rhs is a `type` then I understand that it's better to be less strict here.

> E.g., this is contrived but seems reasonable?
> 
> ```python
> type_of_b = type(b)
> type(a) is type_of_b
> ```
> 
> Pylint flags this under `unidiomatic-typecheck`, but it seems fine to me.

I would write:
```py
type_of_b = type(b)
isinstance(a, type_of_b)
```
Assuming the context is that I'm just dealing with some kind of usual operation where what I really want is a compatible subtype, and that I don't want to be checking for the exact same type.

Isn't the whole point of the inspection to encourage `isinstance` over `==`/`is` for types because `isinstance` is aware of subtypes but `==`/`is` isn't?
```py
class A: ...
class B(A): ...
b = B()
isinstace(b, A)  # True
type(b) == A  # False
```



---

_Comment by @hauntsaninja on 2023-08-08 22:48_

Would you accept a PR with the naive autofix for this?

---

_Comment by @charliermarsh on 2023-08-08 22:58_

Yeah, we can make it a suggested fix.

(Also open to further refinements on the rule logic itself, I’m not certain that our criteria is optimal given above discussion.)

---

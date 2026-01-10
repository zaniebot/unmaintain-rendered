```yaml
number: 6576
title: "ARG002: false negative case with `locals()`"
type: issue
state: closed
author: Olegt0rr
labels:
  - bug
assignees: []
created_at: 2023-08-14T23:23:58Z
updated_at: 2024-07-31T17:08:26Z
url: https://github.com/astral-sh/ruff/issues/6576
synced_at: 2026-01-10T11:09:48Z
```

# ARG002: false negative case with `locals()`

---

_Issue opened by @Olegt0rr on 2023-08-14 23:23_

> ARG002 Unused method argument: `bar`

```python
def foo(bar: str):
    print(locals())
```


---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-14 23:28_

---

_Comment by @charliermarsh on 2023-08-14 23:29_

I guess PyCharm doesn't consider these unused either, so we'll remove.

---

_Label `bug` added by @charliermarsh on 2023-08-14 23:29_

---

_Closed by @charliermarsh on 2023-08-14 23:48_

---

_Comment by @Olegt0rr on 2023-08-15 07:14_

pyCharm works correct with this case:
<img width="251" alt="Screenshot 2023-08-15 at 10 14 33" src="https://github.com/astral-sh/ruff/assets/25399456/8944c698-49f1-42d8-a41b-c985fe1f1bff">

1st `bar` is black, cause used by `locals()`
2nd `bar` is gray, cause unused



---

_Comment by @dycw on 2024-07-31 17:08_

Hi @charliermarsh 

Is it possible to make this an option in that

- `locals()` considers `bar` as used (the current), or
- `locals()` does not consider `bar` as used

I am running into this as I often have an idiom

```
def foo(a, b, c):
    print_or_logging(locals())
    return a + b
```

But now I've missed the fact that `c` is in fact, unused.

---

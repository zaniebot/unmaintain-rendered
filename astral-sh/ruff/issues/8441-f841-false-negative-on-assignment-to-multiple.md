```yaml
number: 8441
title: "`F841` false negative on assignment to multiple variables"
type: issue
state: closed
author: DetachHead
labels:
  - rule
  - help wanted
assignees: []
created_at: 2023-11-02T07:18:21Z
updated_at: 2023-11-05T17:01:11Z
url: https://github.com/astral-sh/ruff/issues/8441
synced_at: 2026-01-10T11:09:50Z
```

# `F841` false negative on assignment to multiple variables

---

_Issue opened by @DetachHead on 2023-11-02 07:18_

```py
def foo():
    a = bar() # error: F841
    b, c = bar() # no error

def bar():
    ...
```
[playground](https://play.ruff.rs/e10d4570-4368-4d83-af88-57b221fe899f)

---

_Comment by @qdegraaf on 2023-11-02 10:29_

Could this be intentional? From [the tests](https://github.com/astral-sh/ruff/blob/d350ede992ae30067486e56852ca106675ddd57d/crates/ruff_linter/resources/test/fixtures/pyflakes/F841_1.py#L1C1-L2C45):

```python
def f(tup):
    x, y = tup  # this does NOT trigger F841
```

Unsure if that was left there as a TODO or consciously omitted. @charliermarsh could you clarify? 

---

_Comment by @charliermarsh on 2023-11-02 12:53_

Yeah this is intentional for parity with Flake8. I think it's reasonable. An improvement could be to flag `x` and `y` if _both_ are unused.

---

_Label `rule` added by @charliermarsh on 2023-11-02 12:53_

---

_Comment by @DetachHead on 2023-11-02 12:59_

I don't get why it should be treated differently. If it's for the case where you only want to use one of the values from the tulle, you can just prefix it with an `_` to mark it as intentionally unused:
```py
a, _b = foo()
```

---

_Comment by @zanieb on 2023-11-02 16:39_

Best practice is really

```
a, _ = foo()
```

I don't see why we wouldn't raise a violation here, other than flake8 compatibility

---

_Comment by @charliermarsh on 2023-11-03 00:13_

I'm not opposed to changing it. I didn't feel strongly so erred towards compatibility.

---

_Label `help wanted` added by @zanieb on 2023-11-03 00:51_

---

_Closed by @charliermarsh on 2023-11-05 17:01_

---

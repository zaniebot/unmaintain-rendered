---
number: 780
title: False positive for F821
type: issue
state: closed
author: olliemath
labels:
  - bug
assignees: []
created_at: 2022-11-16T22:40:20Z
updated_at: 2022-11-26T15:12:08Z
url: https://github.com/astral-sh/ruff/issues/780
synced_at: 2026-01-10T01:22:38Z
---

# False positive for F821

---

_Issue opened by @olliemath on 2022-11-16 22:40_

It seems that using `dict` as a parameter name in a function (which I'd consider bad style) breaks ruff's parsing in unexpected ways. In particular the following gives `6:31: F821 Undefined name 'name'`, when it shouldn't. 

```python
def unimportant(name):
    pass


def dang(dict):
    return unimportant(name=dict["name"])
```

Here is my pyproject.toml if that's needed to reproduce:
```toml
[tool.ruff]
ignore = [
    'B007',
    'E741',
    'E501',
]
line-length = 88
select = [
    'B',
    'E',
    'F',
    'W',
]
```

---

_Label `bug` added by @charliermarsh on 2022-11-16 23:46_

---

_Comment by @charliermarsh on 2022-11-16 23:46_

Thanks! Yeah, definitely a legit bug. We need to verify that `dict` corresponds to the built-in and hasn't been shadowed.

---

_Comment by @charliermarsh on 2022-11-16 23:46_

Appreciate the clear test case.

---

_Comment by @charliermarsh on 2022-11-16 23:52_

(I'll fix this tomorrow most likely, probably not tonight.)

---

_Comment by @charliermarsh on 2022-11-17 00:09_

Tangentially, I guess I'd argue that Flake8 erroneously _doesn't_ catch this:

```py
# Flake8 allows this
x = dict["foo"]

# Flake8 raises F821 here
x: dict["foo"]
```

Both should raise an undefined error on `foo`.


---

_Comment by @charliermarsh on 2022-11-17 00:10_

The reason I mention it is because both Flake8 and Ruff accidentally raise F821 here:

```py
def f(dict):
    x: dict["foo"] = 1
```

---

_Comment by @charliermarsh on 2022-11-17 00:12_

But I will fix this in Ruff :)

---

_Comment by @olliemath on 2022-11-17 09:44_

Thanks for the quick response! Yeah interesting that flake8 has that issue too - I guess it's a bit more niche, so as-yet undiscovered (or unfixed) by them.

---

_Comment by @olliemath on 2022-11-17 10:49_

Well, I've also let the pyflakes team know about this edge case https://github.com/PyCQA/pyflakes/issues/738 I assume they'll want to fix it!

---

_Comment by @olliemath on 2022-11-17 13:40_

> The reason I mention it is because both Flake8 and Ruff accidentally raise F821 here:
> 
> ```python
> def f(dict):
>     x: dict["foo"] = 1
> ```

So pyflakes adopt the attitude of "this is a bad type annotation - won't fix". In fact this also gives F821 with flake8 but not ruff:
```python
def f(noshadow):
    x: noshadow["foo"] = 1
```

Either way, having consistent behavior in ruff with or without shadowing is good (whether or not that differs from flake8 slightly).

---

_Comment by @charliermarsh on 2022-11-18 17:30_

I'm behind on this, it'll come soon :)

---

_Referenced in [astral-sh/ruff#787](../../astral-sh/ruff/issues/787.md) on 2022-11-18 18:55_

---

_Comment by @JonathanPlasse on 2022-11-19 06:15_

@charliermarsh, I could work on it if it is ok with you.

---

_Comment by @charliermarsh on 2022-11-20 15:53_

That'd be great @JonathanPlasse! I know it's relevant to the `sys.exit` stuff too.

---

_Referenced in [astral-sh/ruff#911](../../astral-sh/ruff/pulls/911.md) on 2022-11-26 12:56_

---

_Closed by @charliermarsh on 2022-11-26 15:12_

---

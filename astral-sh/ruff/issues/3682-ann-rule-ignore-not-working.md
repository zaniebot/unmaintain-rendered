---
number: 3682
title: "ANN* rule ignore not working"
type: issue
state: closed
author: silviumarcu
labels:
  - bug
assignees: []
created_at: 2023-03-23T10:33:49Z
updated_at: 2023-03-23T17:04:34Z
url: https://github.com/astral-sh/ruff/issues/3682
synced_at: 2026-01-10T01:22:42Z
---

# ANN* rule ignore not working

---

_Issue opened by @silviumarcu on 2023-03-23 10:33_

When adding `ANN*` (e.g.  ANN101, ANN102) to either `ignore` or `extend-ignore` errors are still displayed.

 `ruff_lab.py`
```
class Test:
    def do_stuff(self) -> None:
        print('stuff')

    @classmethod
    def do_more_stuff(cls) -> None:
        print("more stuff")
```

`pyproject.toml`
```
[tool.ruff]
extend-select = ["ANN"]
ignore = ["ANN102", "ANN101"]
```

```
pip freeze | grep ruff
ruff==0.0.258
```

```
ruff check ruff_lab.py
ruff_lab.py:2:18: ANN101 Missing type annotation for `self` in method
ruff_lab.py:6:23: ANN102 Missing type annotation for `cls` in classmethod
Found 2 errors.
```

When downgrading to `0.0.257` the behavior is the expected one.



---

_Comment by @JonathanPlasse on 2023-03-23 12:01_

Did you try `select=ANN`?

---

_Comment by @silviumarcu on 2023-03-23 13:51_

Adding `select = ["ANN"]` in `pyproject.toml` seems to work. Is `extended_select` deprecated?

---

_Comment by @JonathanPlasse on 2023-03-23 13:55_

No, but `extend_select` take priority over `ignore`.
This is to allow to override ignore in the CLI.

---

_Comment by @charliermarsh on 2023-03-23 17:03_

I suspect this is a bug actually, especially if it worked on `0.0.257`. I would expect the config above to work as you expect it to. (`extend-select` just extends `select`, it doesn't have higher precedence than `ignore`.)

---

_Label `bug` added by @charliermarsh on 2023-03-23 17:03_

---

_Comment by @charliermarsh on 2023-03-23 17:04_

Just confirmed that this is fixed by https://github.com/charliermarsh/ruff/pull/3685, which'll go out in a new release today.

---

_Closed by @charliermarsh on 2023-03-23 17:04_

---

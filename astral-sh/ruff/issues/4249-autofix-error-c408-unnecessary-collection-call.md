```yaml
number: 4249
title: "[Autofix error] C408 - unnecessary-collection-call - syntax error when fixing within f-string"
type: issue
state: closed
author: Hnasar
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-05-05T18:00:01Z
updated_at: 2023-06-09T04:53:15Z
url: https://github.com/astral-sh/ruff/issues/4249
synced_at: 2026-01-12T15:54:44Z
```

# [Autofix error] C408 - unnecessary-collection-call - syntax error when fixing within f-string

---

_@Hnasar_

Presumably replacing `dict()` with `{}` gets confused when inside an f-string when it comes to using `"` vs `'` (or maybe it's the nested `{`)

```python
# test.py
f"{dict(x='y')}"  # should turn into f"{{'x': 'y'}}"
```

```
$ ruff --select=C408 test.py  --fix

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `test.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

test.py:1:4: C408 Unnecessary `dict` call (rewrite as a literal)
```

Using ruff 0.0.264


---

_Label `bug` added by @charliermarsh on 2023-05-05 18:02_

---

_Comment by @charliermarsh on 2023-05-05 18:02_

Good call, thanks!

---

_Label `good first issue` added by @charliermarsh on 2023-05-05 18:05_

---

_Comment by @charliermarsh on 2023-05-05 18:06_

(We track `in_f_string`, so we should just avoid triggering this rule, and perhaps some others in this category, when `in_f_string` is `true`.)

---

_Comment by @dhruvmanila on 2023-05-06 11:55_

I think it's the braces and not the quotes as the quotes are taken as is. So, this should be avoided in `dict` and `set` comprehensions.

---

_Comment by @akx on 2023-05-09 10:40_

Maybe the heuristics in the new `ruff::rules::flynt::helpers::to_fstring_elem` function could be of more generic use here :)

---

_Comment by @DavideCanton on 2023-05-21 14:25_

I can work on it if nobody else wants to

---

_Assigned to @DavideCanton by @MichaReiser on 2023-05-21 18:28_

---

_Closed by @charliermarsh on 2023-06-09 04:53_

---

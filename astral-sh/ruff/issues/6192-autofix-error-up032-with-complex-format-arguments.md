```yaml
number: 6192
title: "[Autofix error] UP032 with complex format arguments"
type: issue
state: closed
author: tsx
labels:
  - bug
assignees: []
created_at: 2023-07-31T10:45:02Z
updated_at: 2023-07-31T13:12:40Z
url: https://github.com/astral-sh/ruff/issues/6192
synced_at: 2026-01-10T11:09:48Z
```

# [Autofix error] UP032 with complex format arguments

---

_Issue opened by @tsx on 2023-07-31 10:45_

Reproduction:

```python
some_variable = "{xyz} {abc}".format(
    xyz=some_function(
        this_argument_has_to_be_on_a_separate_line_very_long_or_multiple_arguments
    ),
    abc=abc,
)
```

```bash
$ ruff --isolated --select UP032 --fix example.py

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `example.py`, the rule codes UP032, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

example.py:1:17: UP032 Use f-string instead of `format` call
```

This is affecting 0.0.279 and 0.0.280, but 0.0.278 is fine.

Relevant settings:
```
[tool.ruff]
target-version = "py310"
line-length = 79
```



---

_Comment by @charliermarsh on 2023-07-31 13:12_

I believe this is fixed on `main` (and should be resolved in today's release, v0.0.281). Sorry about that.

---

_Label `bug` added by @charliermarsh on 2023-07-31 13:12_

---

_Closed by @charliermarsh on 2023-07-31 13:12_

---

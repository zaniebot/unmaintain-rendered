```yaml
number: 6610
title: "noqa: E501 is incorrectly parsed at the end of a docstring"
type: issue
state: closed
author: mscheltienne
labels:
  - bug
assignees: []
created_at: 2023-08-16T08:34:52Z
updated_at: 2023-08-16T09:41:52Z
url: https://github.com/astral-sh/ruff/issues/6610
synced_at: 2026-01-12T15:54:46Z
```

# noqa: E501 is incorrectly parsed at the end of a docstring

---

_@mscheltienne_

Hello, our CIs updated to a newer Ruff version and are now failing on an E501 error, marked with `noqa: E501`.

```
def foo(...):
    """
    super neat docstring with a long line (who does not love URLs)
    more lines
    ...
    """  # noqa: E501
    pass
```

The command `ruff check path/to/the/file.py` returns:

```
warning: Invalid `# noqa` directive on pycrostates/datasets/lemon/lemon.py:68: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
pycrostates/datasets/lemon/lemon.py:68:89: E501 Line too long (152 > 88 characters)
Found 1 error.
```

So for some reason, it's incorrectly parsed. 
Version: 0.0.284
`pyproject.toml`:

```
[tool.ruff]
line-length = 88
extend-exclude = [
    "doc",
    "setup.py",
]

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
```

For x-ref to our project issue tracker: https://github.com/vferat/pycrostates/pull/115

---

_Label `bug` added by @MichaReiser on 2023-08-16 08:43_

---

_Comment by @mscheltienne on 2023-08-16 09:41_

Actually my bad, I misread the line number and missed the other badly formatted `noqa`.. 

---

_Closed by @mscheltienne on 2023-08-16 09:41_

---

```yaml
number: 6037
title: "Rule D413 produces a false positive for a section headed `Examples`."
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2023-07-24T15:48:02Z
updated_at: 2023-07-24T16:37:25Z
url: https://github.com/astral-sh/ruff/issues/6037
synced_at: 2026-01-12T15:54:45Z
```

# Rule D413 produces a false positive for a section headed `Examples`.

---

_@ghost_

The bug is exhibited by the command
```shell
ruff --isolated --select D413 /path/to/file.py
```

If `file.py` is
```python
"""Title.

Examples
--------
Body.
"""
```
there is a false positive:
```
file.py:1:1: D413 [*] Missing blank line after last section ("Examples")
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

However, there is, correctly, no error detected if `file.py` is
```python
"""Title.

Heading
-------
Body.
"""
```

Running `ruff --version` prints `ruff 0.0.280`.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-24 15:50_

---

_Comment by @charliermarsh on 2023-07-24 15:55_

So I _think_ this is correct. This rule wants you to format like:

```py
"""Title.

Examples
--------
Body.

"""
```

Which is consistent with pydocstyle's own implementation of this rule.

If you're looking to guard against:

```py
"""Title.

Examples
--------
Body."""
```

That's instead covered by D209.

Note that this is kind of a strange rule -- it's not included in the NumPy, Google, or PEP 257 conventions.

---

_Closed by @charliermarsh on 2023-07-24 15:55_

---

_Comment by @ghost on 2023-07-24 16:24_

@charliermarsh,

Firstly, there is only one new line at the end of the docstring given as an example of the desired style in the documentation for the rule.
https://beta.ruff.rs/docs/rules/blank-line-after-last-section/#__code_1

Secondly, if the rule ought to require two new lines there is a false negative when `file.py` is
```python
"""Title.

Heading
-------
Body.
"""
```

---

_Comment by @charliermarsh on 2023-07-24 16:27_

Thanks, I think that documentation is just wrong. I'll fix it.

I believe that "Heading" is not a recognized section header (unlike "Examples"), `pydocstyle` doesn't error there either.

---

_Comment by @ghost on 2023-07-24 16:29_

What headings are recognized?

---

_Comment by @charliermarsh on 2023-07-24 16:31_

```python
[
    "Args",
    "Arguments",
    "Attention",
    "Attributes",
    "Caution",
    "Danger",
    "Error",
    "Example",
    "Examples",
    "Extended Summary",
    "Hint",
    "Important",
    "Keyword Args",
    "Keyword Arguments",
    "Methods",
    "Note",
    "Notes",
    "Other Args",
    "Other Arguments",
    "Other Params",
    "Other Parameters",
    "Parameters",
    "Raises",
    "References",
    "Return",
    "Returns",
    "See Also",
    "Short Summary",
    "Tip",
    "Todo",
    "Warning",
    "Warnings",
    "Warns",
    "Yield",
    "Yields",
]
```

---

_Comment by @ghost on 2023-07-24 16:32_

Thank you.

---

_Comment by @charliermarsh on 2023-07-24 16:37_

I can add something to the FAQ around this point.

---

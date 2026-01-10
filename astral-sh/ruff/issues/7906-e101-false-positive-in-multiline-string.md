```yaml
number: 7906
title: E101 False positive in multiline string
type: issue
state: open
author: david-waterworth
labels:
  - needs-info
  - needs-decision
assignees: []
created_at: 2023-10-11T01:21:25Z
updated_at: 2024-09-13T22:16:44Z
url: https://github.com/astral-sh/ruff/issues/7906
synced_at: 2026-01-10T11:09:50Z
```

# E101 False positive in multiline string

---

_Issue opened by @david-waterworth on 2023-10-11 01:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have a python model that contains rest endpoint queries defined as multi-line strings, i.e. 

```
query = """
    query ...
"""
```

`ruff` is reporting `E101 Indentation contains mixed spaces and tabs` for the text inside the string.

Technically this is correct as the indentation of the query does contain a mix of tab and spaces - but in general, I feel that `ruff` shouldn't try and lint a text block.

Note I observed this in a notebook - I've not tested in a py file

---

_Comment by @charliermarsh on 2023-10-11 03:46_

So pycodestyle has this behavior too, though it's unclear whether they consider it correct (https://github.com/PyCQA/pycodestyle/issues/600, https://github.com/PyCQA/pycodestyle/issues/376, https://github.com/PyCQA/pycodestyle/issues/45).

We actually disabled W109 (which flags uses of tabs) within such strings. I'm open to disabling E101 too -- but what's the valid use-case for mixing spaces and tabs within a single indent, even within a string?

---

_Label `needs-decision` added by @charliermarsh on 2023-10-11 03:46_

---

_Label `waiting-on-author` added by @charliermarsh on 2023-10-12 16:11_

---

_Comment by @njzjz on 2024-09-13 19:01_

In a unit test, one may want to check if a file has the expected content.

```py
a = """
	    example1
	    example2
"""
with open("some_file.txt") as f:
    b = f.read()
assert a == b
```

The fix breaks the test.

---

_Comment by @MichaReiser on 2024-09-13 22:16_

Ignoring leading whitespace in multiline strings seems reasonable because it is not an indentation. 

---

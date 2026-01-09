---
number: 7121
title: Rule C417 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-04T05:59:19Z
updated_at: 2023-09-06T12:56:49Z
url: https://github.com/astral-sh/ruff/issues/7121
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule C417 cause autofix error

---

_Issue opened by @qarmin on 2023-09-04 05:59_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select C417 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
if cls:
        entries_list_xslt_tree = etree.XSLT(
            entries_list_xslt, extensions={
                ("entries_list_ns", "EntryName"): EntryName})
        entries_list_xslt_tree(self, **dict(zip(
            map(lambda x: etree.XSLT.strparam(str(x)),
                limits if limits is not None else [0x0000, 0xFFFF])
            )))
```

error
```
/home/rafal/test/tmp_folder/197731.py:6:13: C417 Unnecessary `map` usage (rewrite using a generator expression)
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/197731.py`, the rule codes C417, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12509752/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-04 06:27_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-04 06:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-06 11:53_

---

_Referenced in [astral-sh/ruff#7189](../../astral-sh/ruff/pulls/7189.md) on 2023-09-06 12:37_

---

_Closed by @charliermarsh on 2023-09-06 12:56_

---

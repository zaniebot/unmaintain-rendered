---
number: 8090
title: Black deviation with if x in some comprehension 
type: issue
state: closed
author: philippgl
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-20T09:09:48Z
updated_at: 2023-10-22T23:58:27Z
url: https://github.com/astral-sh/ruff/issues/8090
synced_at: 2026-01-07T13:12:15-06:00
---

# Black deviation with if x in some comprehension 

---

_Issue opened by @philippgl on 2023-10-20 09:09_

I just fund a small difference with the black formatter. 

* A minimal code snippet that reproduces the bug.

This is the black formatted example (imho, this also looks nicer):

```python
def some_function():
    if "root" not in (
        long_tree_name_tree.split("/")[0]
        for long_tree_name_tree in really_really_long_variable_name
    ):
        msg = "Could not find root. Please try a different forest."
        raise ValueError(msg)
```

which gets reformatted to:

```python
def some_function():
    if (
        "root"
        not in (
            long_tree_name_tree.split("/")[0]
            for long_tree_name_tree in really_really_long_variable_name
        )
    ):
        msg = "Could not find root. Please try a different forest."
        raise ValueError(msg)
```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```sh
ruff format --isolated filename.py
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

None. This is a standalone script.

* The current Ruff version (`ruff --version`).

ruff 0.0.292
ruff 0.1.0

(I tried it with both versions)


---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-20 09:23_

---

_Label `formatter` added by @MichaReiser on 2023-10-20 09:23_

---

_Comment by @MichaReiser on 2023-10-20 09:28_

Yeah, Black's output certainly looks better. I think the issue is that our `can_omit_optional_parentheses` returns false for the condition where Black returns `true`

---

_Comment by @charliermarsh on 2023-10-20 20:40_

I'll take a quick look here.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-20 20:47_

---

_Label `bug` added by @charliermarsh on 2023-10-20 20:47_

---

_Referenced in [astral-sh/ruff#8100](../../astral-sh/ruff/pulls/8100.md) on 2023-10-20 21:11_

---

_Comment by @charliermarsh on 2023-10-20 21:15_

Thanks for reporting, great issue.

---

_Closed by @charliermarsh on 2023-10-22 23:58_

---

_Referenced in [astral-sh/ruff#8183](../../astral-sh/ruff/issues/8183.md) on 2023-10-25 09:00_

---

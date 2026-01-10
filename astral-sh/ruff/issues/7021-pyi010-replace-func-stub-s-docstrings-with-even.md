---
number: 7021
title: "PYI010: replace func stub's docstrings with '...' even if there already is one."
type: issue
state: closed
author: Prometheos2
labels:
  - bug
assignees: []
created_at: 2023-08-31T14:08:15Z
updated_at: 2023-08-31T20:45:27Z
url: https://github.com/astral-sh/ruff/issues/7021
synced_at: 2026-01-10T01:22:46Z
---

# PYI010: replace func stub's docstrings with '...' even if there already is one.

---

_Issue opened by @Prometheos2 on 2023-08-31 14:08_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello,

I've noticed PYI010's autofix replaces function stubs' docstrings with ellipsis regardless of if there is one already or not.

### Minimal code snippet

```python
def function():
    """doc"""
    ...
```
### Details
* **The commands I invoked:** 
  * the VSC extension's autofix
  * `ruff . --fix`
  * `ruff . --fix --isolated --select "PYI"` (got a `error: Failed to converge after 100 iterations.` with this one; ~~will create a new issue soon~~)

* **The current Ruff version:** 0.0.286

### The current Ruff settings
* A _ruff.toml_ file on the project root (linted file is in a _typings_ subfolder)
```toml
select = [
	# ...
	"PYI",  # flake8-pyi
	# ...
]
include = ["*.py", "*.pyi", "*.ipynb", "pyproject.toml", "rust.toml"]
ignore = ["E501", "N999"]
target-version = "py38"
src = ["Phonebook_analyzer"]
```
 * Added a `# "PYI010"` in unfixable after the first command; no sure if it's relevant.   



Thanks for the linter, btw; it's really awesome! 


---

_Label `bug` added by @charliermarsh on 2023-08-31 14:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-31 14:43_

---

_Comment by @charliermarsh on 2023-08-31 14:43_

Thanks for reporting :)

---

_Comment by @Prometheos2 on 2023-08-31 14:47_

You're welcome ^^

Do you want me to report the infinite loop? I think it might be due to PYI010 replacing the second ellipsis with an ellipsis

---

_Comment by @charliermarsh on 2023-08-31 14:48_

No thanks, this issue is sufficient. I need to look into the relationship between a few related rules here. (We also may need to remove the autofix here, it seems like it's going to be wrong most of the time.)

---

_Referenced in [astral-sh/ruff#7024](../../astral-sh/ruff/pulls/7024.md) on 2023-08-31 15:14_

---

_Closed by @charliermarsh on 2023-08-31 20:45_

---

```yaml
number: 11477
title: "C901: The `max-complexity` linting rule does not appear to work"
type: issue
state: closed
author: grahamlyons
labels: []
assignees: []
created_at: 2024-05-20T14:12:30Z
updated_at: 2024-05-20T14:16:59Z
url: https://github.com/astral-sh/ruff/issues/11477
synced_at: 2026-01-12T15:54:51Z
```

# C901: The `max-complexity` linting rule does not appear to work

---

_@grahamlyons_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Adding the following into a `ruff.toml` config file and running `ruff check -v` against the file produces no violations, whereas `flake8` with `mccabe` flags the code as too complex.

```toml
[lint.mccabe]
# Flag errors (`C901`) whenever the complexity level exceeds 5.
max-complexity = 3
```

Example code:
```python
def foo(a, b, c):
    if a:
        if b:
            if c:
                return 1
            else:
                return 2
        else:
            return 3
    else:
        return 4
```

Output from `flake8`:
```shell
# flake8 --max-complexity=3 test.py 
test.py:2:1: C901 'foo' is too complex (4)
```

Output from `ruff`:
```shell
# ruff check test.py  -v
[2024-05-20][08:47:12][ruff::resolve][DEBUG] Using configuration file (via parent) at: /workspaces/ruff-test/ruff.toml
[2024-05-20][08:47:12][ruff_workspace::pyproject][DEBUG] `project.requires_python` in `pyproject.toml` will not be used to set `target_version` when using `ruff.toml`.
[2024-05-20][08:47:12][ruff::commands::check][DEBUG] Identified files to lint in: 2.615875ms
[2024-05-20][08:47:12][ruff::commands::check][DEBUG] Checked 1 files in: 31.084µs
All checks passed!
```

Ruff version:
`ruff --version: ruff 0.4.4`

This looks to be a bug but if I'm doing something wrong then please let me know.

---

_Comment by @charliermarsh on 2024-05-20 14:13_

Did you add `C901` to the list of enabled rules? That rule isn't enabled by default. Something like:

```toml
[lint]
extend-select = ["C901"]
```

---

_Comment by @grahamlyons on 2024-05-20 14:16_

Thanks for the quick response - that was the problem.

---

_Closed by @grahamlyons on 2024-05-20 14:16_

---

_Comment by @charliermarsh on 2024-05-20 14:16_

No prob.

---

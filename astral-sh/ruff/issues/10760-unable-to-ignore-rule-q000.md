---
number: 10760
title: Unable to ignore rule Q000
type: issue
state: closed
author: kohlerjl
labels: []
assignees: []
created_at: 2024-04-03T18:12:14Z
updated_at: 2024-04-03T18:27:44Z
url: https://github.com/astral-sh/ruff/issues/10760
synced_at: 2026-01-10T01:22:50Z
---

# Unable to ignore rule Q000

---

_Issue opened by @kohlerjl on 2024-04-03 18:12_

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

Using the latest Ruff version (0.3.5), my configuration to ignore rule Q000 ("Single quotes found but double quotes preferred") is no longer respected. 

Here's an example file `test.py`
``` python
test = 'hello, world'
```

And configuration `ruff.toml`
``` toml
[lint]

select = [
   "Q", # flake8-quotes
]

ignore = [
    "Q000",  # Single quotes found but double quotes preferred
]
```

Using version 0.3.5 I get output:
```
(main) C:\Users\VA\test>ruff version
ruff 0.3.5 (200ebeebd 2024-04-01)

(main) C:\Users\VA\test>ruff check
test.py:1:8: Q000 Single quotes found but double quotes preferred
Found 1 error.
```

This only occurs on version 0.3.5, and is not present in 0.3.4
```
(main) C:\Users\VA\test>ruff version
ruff 0.3.4 (5062572ac 2024-03-21)

(main) C:\Users\VA\test>ruff check
All checks passed!
```




---

_Comment by @AlexWaygood on 2024-04-03 18:27_

Thanks for opening the issue! Luckily this has already been fixed on the `main` branch, so the fix will definitely be included in the next release :-)

Closing as a duplicate of #10724

---

_Closed by @AlexWaygood on 2024-04-03 18:27_

---

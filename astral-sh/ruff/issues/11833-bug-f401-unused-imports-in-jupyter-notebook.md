```yaml
number: 11833
title: Bug - F401 Unused imports in Jupyter Notebook breaks shell commands
type: issue
state: closed
author: jeertmans
labels:
  - bug
  - notebook
assignees: []
created_at: 2024-06-11T07:32:14Z
updated_at: 2024-06-11T12:24:15Z
url: https://github.com/astral-sh/ruff/issues/11833
synced_at: 2026-01-12T15:54:51Z
```

# Bug - F401 Unused imports in Jupyter Notebook breaks shell commands

---

_@jeertmans_

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

Hello!

First of all, thanks for the great tool!

Inside notebooks (e.g., Google Colab), it is very common, and [recommended](https://jakevdp.github.io/blog/2017/12/05/installing-python-packages-from-jupyter/), to run `pip install` Python packages by linking to the appropriate Python executable, which hence needs `sys.executable`, see code snippet below.

However, Ruff will not detect that `sys` is needed in the shell command (with `!`) and will remove the `import sys` line, which will ultimately break the notebook.

Without `--fix`, the output is:

```bash
notebook.ipynb:cell 2:7:12: F401 [*] `sys` imported but unused
```


**Keywords:** "Ruff", "F401", "Jupyter", "Notebook", "unused import", "shell command".

Minimal code snippet:

```python
try:
    import package  # noqa: F401
except ImportError:
    import sys
    !{sys.executable} -m pip install package
```

A temporary fix is to add disable this check on `import sys`:

```python
try:
    import package  # noqa: F401
except ImportError:
    import sys  # noqa: F401
    !{sys.executable} -m pip install package
```

While this fix is fine, I had to learn it the hard way, as the `unused-import` fix is considered stable and will not warn you about this if you directly run with `--fix`.

---

_Comment by @tdulcet on 2024-06-11 11:52_

Same type of issue as #8094.

---

_Comment by @jeertmans on 2024-06-11 12:22_

Indeed @tdulcet, couldn't find it :-)

---

_Comment by @charliermarsh on 2024-06-11 12:23_

👍 I'm going to combine with #8094. Thanks!

---

_Closed by @charliermarsh on 2024-06-11 12:24_

---

_Label `bug` added by @charliermarsh on 2024-06-11 12:24_

---

_Label `notebook` added by @charliermarsh on 2024-06-11 12:24_

---

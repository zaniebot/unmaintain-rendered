```yaml
number: 11368
title: TCH005 autofix deletes else block, removes functional code.
type: issue
state: closed
author: Skylion007
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-05-11T14:49:45Z
updated_at: 2024-05-13T00:22:27Z
url: https://github.com/astral-sh/ruff/issues/11368
synced_at: 2026-01-10T11:09:53Z
```

# TCH005 autofix deletes else block, removes functional code.

---

_Issue opened by @Skylion007 on 2024-05-11 14:49_

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
I applied TCH autofixes to pytorch recently, and hit a pretty gnarly false positive here.
In this case `TCH005` TYPE_CHECKING "pass" is used to trigger the else block. When the autofix is applied, instead of doing something sensible like changing it to an `if not TYPE_CHECKING` it just deletes the entire block. Here is the commit ID that reverted it:
https://github.com/pytorch/pytorch/pull/125536/commits/b7f8f56a079399a32129e3fcc424ae88202baea5#diff-5f3d4caa0693a716fc46fd7f6339312f1b5f0bf89e3a3ff58e9dc13a9486b17aR1103

```python
if TYPE_CHECKING:
   pass
else:
   print("IMPORTANT PRINT STATEMENT HERE")
```
the fixit would delete the entire code block instead of doing a more sensible fix.

---

_Label `fixes` added by @AlexWaygood on 2024-05-11 14:55_

---

_Label `bug` added by @charliermarsh on 2024-05-11 16:17_

---

_Closed by @charliermarsh on 2024-05-13 00:22_

---

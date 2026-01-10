```yaml
number: 7216
title: "D411: Missing blank line before section -- false positive"
type: issue
state: closed
author: pepoluan
labels:
  - docstring
assignees: []
created_at: 2023-09-07T10:25:46Z
updated_at: 2023-09-13T16:41:14Z
url: https://github.com/astral-sh/ruff/issues/7216
synced_at: 2026-01-10T11:09:49Z
```

# D411: Missing blank line before section -- false positive

---

_Issue opened by @pepoluan on 2023-09-07 10:25_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
The code looks like this:

```python
class JobProgress:
    """
    Tracks progress by generating job batches and recording the last issued job.
    
    Attributes
    ----------
        next_coordinate(int):
    """
```

Ruff <kbd>D411</kbd> complains that a blank line must precede the `Attributes` section.

But as you can see, there *is* actually a blank line ... though it actually contains 4 spaces, this is due to the auto-indent feature of my IDE (PyCharm).

Such a line (containing only spaces) should not trigger <kbd>D411</kbd> because visually they are the same.

I am using **ruff 0.0.277** on Windows.


---

_Label `docstring` added by @charliermarsh on 2023-09-12 15:47_

---

_Closed by @charliermarsh on 2023-09-13 16:41_

---

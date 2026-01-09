---
number: 9145
title: "[Panic] byte index 2 is not a char boundary"
type: issue
state: closed
author: yuuuxt
labels:
  - bug
assignees: []
created_at: 2023-12-15T06:32:36Z
updated_at: 2023-12-15T14:15:48Z
url: https://github.com/astral-sh/ruff/issues/9145
synced_at: 2026-01-07T13:12:15-06:00
---

# [Panic] byte index 2 is not a char boundary

---

_Issue opened by @yuuuxt on 2023-12-15 06:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

using `v2023.56.0` and `1.85.1` in win10.

Seems a line inside docstring that starts with a Chinese character would cause this. 

In jupyter notebook I create a function:

```python
def sample_func(xx):
    """
    转置 (transpose)
    """
    return xx.T

```

the error logs:

```
thread 'main' panicked at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1\library\core\src\str\mod.rs:660:13:
byte index 2 is not a char boundary; it is inside '转' (bytes 0..3) of `转置`
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @MichaReiser on 2023-12-15 07:46_

What's the command you're running? Is it `ruff check` or `ruff format`?

---

_Label `bug` added by @MichaReiser on 2023-12-15 07:46_

---

_Comment by @yuuuxt on 2023-12-15 08:25_

not running any command, it's like an auto-check (not manually triggering formatting), so I assume it's similar to running `ruff check`.

---

_Comment by @manunio on 2023-12-15 08:27_

@MichaReiser I think the bug is for both `check` and `format` , I was able to repro this by running check against .ipynb file with above sample_func.

```
ruff check .\test.ipynb
error: Panicked while linting test.ipynb: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1\library\core\src\str\mod.rs:660:13:
byte index 2 is not a char boundary; it is inside '转' (bytes 0..3) of `转置`
```



---

_Assigned to @konstin by @konstin on 2023-12-15 08:47_

---

_Referenced in [astral-sh/ruff#9146](../../astral-sh/ruff/pulls/9146.md) on 2023-12-15 09:19_

---

_Closed by @dhruvmanila on 2023-12-15 14:15_

---

_Referenced in [astral-sh/ruff#9199](../../astral-sh/ruff/pulls/9199.md) on 2023-12-19 13:09_

---

_Referenced in [astral-sh/ruff#9225](../../astral-sh/ruff/issues/9225.md) on 2023-12-21 09:55_

---

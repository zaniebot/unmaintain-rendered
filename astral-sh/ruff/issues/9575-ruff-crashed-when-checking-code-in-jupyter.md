```yaml
number: 9575
title: Ruff crashed when checking code in jupyter notebook. It failed to handle non-ascii characters correcly.
type: issue
state: closed
author: LENAX
labels:
  - question
assignees: []
created_at: 2024-01-19T04:02:11Z
updated_at: 2024-01-24T06:14:18Z
url: https://github.com/astral-sh/ruff/issues/9575
synced_at: 2026-01-12T15:54:49Z
```

# Ruff crashed when checking code in jupyter notebook. It failed to handle non-ascii characters correcly.

---

_@LENAX_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Ruff: Lint failed (
error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1\library\core\src\str\mod.rs:660:13:
byte index 2 is not a char boundary; it is inside '且' (bytes 0..3) of `且类型为枚举SupportType中的类型`
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
)

![image](https://github.com/astral-sh/ruff/assets/1931031/4ae4ef0f-0a9a-4ea1-9e68-eb97526a33fc)



---

_Comment by @MichaReiser on 2024-01-19 08:56_

Hy @LENAX I'm sorry you ran into this issue. What ruff version are you using? It should be fixed in ruff 0.1.9 or newer (see https://github.com/astral-sh/ruff/pull/9146)

---

_Label `question` added by @charliermarsh on 2024-01-19 16:36_

---

_Comment by @LENAX on 2024-01-24 06:14_

I was using version 0.1.8. Thanks for reminding me to upgrade.

---

_Closed by @LENAX on 2024-01-24 06:14_

---

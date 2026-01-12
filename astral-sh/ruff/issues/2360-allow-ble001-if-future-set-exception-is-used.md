```yaml
number: 2360
title: Allow BLE001 if future.set_exception is used
type: issue
state: closed
author: JonathanPlasse
labels:
  - rule
assignees: []
created_at: 2023-01-30T19:05:30Z
updated_at: 2023-05-20T21:48:11Z
url: https://github.com/astral-sh/ruff/issues/2360
synced_at: 2026-01-12T15:54:42Z
```

# Allow BLE001 if future.set_exception is used

---

_@JonathanPlasse_

```python
try:
    client.loop_read()
except Exception as exc:
    if not self._disconnected.done():
        self._disconnected.set_exception(exc)
```
In [asyncio-mqtt](https://github.com/sbtinstruments/asyncio-mqtt/), we use `asyncio.Future.set_exception(exc)` to handle the exception.
- Would it be ok to allow it the same as #2212?

I can work on it.
We could use a visitor as mentioned [here](https://github.com/charliermarsh/ruff/issues/1119#issuecomment-1341171085) to handle it.
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-01-30 20:01_

That seems reasonable, but we should probably also consider whether it affects any of the `TRY` rules and should be allowed there too.

---

_Label `rule` added by @charliermarsh on 2023-01-31 23:07_

---

_Closed by @JonathanPlasse on 2023-05-20 21:48_

---

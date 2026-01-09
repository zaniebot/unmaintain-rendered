---
number: 8198
title: "Minor: slightly misleading error when network is flaky"
type: issue
state: open
author: callegar
labels: []
assignees: []
created_at: 2024-10-15T07:20:32Z
updated_at: 2024-10-15T07:20:32Z
url: https://github.com/astral-sh/uv/issues/8198
synced_at: 2026-01-07T13:12:17-06:00
---

# Minor: slightly misleading error when network is flaky

---

_Issue opened by @callegar on 2024-10-15 07:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Using uv 0.4.21, when something cannot be downloaded, uv fails to say so clearly, and complains that something cannot be extracted from an archive. E.g.

```
uv python install 3.13t
Searching for Python versions matching: Python 3.13t
cpython-3.13.0+freethreaded-linux-x86_64-gnu ------------------------------ 3.30 MiB/46.83 MiB                                                                                           error: Failed to install cpython-3.13.0+freethreaded-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.13.0%2B20241008-x86_64-unknown-linux-gnu-freethreaded%2Bdebug-full.tar.zst
  Caused by: failed to unpack `/home/callegar/.local/share/uv/python/.cache/.tmppoQYXt/python/build/Modules/_sqlite/cursor.o`
  Caused by: failed to unpack `python/build/Modules/_sqlite/cursor.o` into `/home/callegar/.local/share/uv/python/.cache/.tmppoQYXt/python/build/Modules/_sqlite/cursor.o`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: operation timed out
```

Would probably be better to say something like
```
error: Failed to download cpython-3.13.0%2B20241008-x86_64-unknown-linux-gnu-freethreaded%2Bdebug-full.tar.zst
```
and stop there.


---

_Referenced in [astral-sh/uv#8525](../../astral-sh/uv/issues/8525.md) on 2024-10-24 12:12_

---

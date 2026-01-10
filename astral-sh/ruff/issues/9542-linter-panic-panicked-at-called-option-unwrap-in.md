```yaml
number: 9542
title: "[Linter panic] panicked at 'called `Option::unwrap()` in pycodestyle invalid_escape_sequence.rs"
type: issue
state: closed
author: rsyring
labels: []
assignees: []
created_at: 2024-01-16T03:37:25Z
updated_at: 2024-01-17T17:33:26Z
url: https://github.com/astral-sh/ruff/issues/9542
synced_at: 2026-01-10T11:09:51Z
```

# [Linter panic] panicked at 'called `Option::unwrap()` in pycodestyle invalid_escape_sequence.rs

---

_Issue opened by @rsyring on 2024-01-16 03:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Possibly a dupe of #9398

```python
# Code
print(f'Found template at {template.origin} for locations: {%s', template.origin, template_list)
```

```toml
# pyproject.toml
[tool.ruff]
select = [
    'W',   # pycodestyle warnings
]
```

Error:
```
panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs:175:18
Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  19: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  20: <unknown>
```

---

_Comment by @rsyring on 2024-01-16 03:41_

I thought I had the latest version of Ruff installed but upon further review, it was v0.1.0.  Upgrading to 0.1.13 resolved the crash.

---

_Closed by @rsyring on 2024-01-16 03:41_

---

_Comment by @charliermarsh on 2024-01-16 03:42_

Oh great, thank you!

---

_Comment by @dhruvmanila on 2024-01-17 17:33_

Just for reference this was fixed in https://github.com/astral-sh/ruff/pull/8154 (`v0.1.4`).

---

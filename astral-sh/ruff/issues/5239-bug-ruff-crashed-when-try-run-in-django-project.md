```yaml
number: 5239
title: "[BUG] Ruff crashed when try run in django project with poetry"
type: issue
state: closed
author: moreiralucas
labels: []
assignees: []
created_at: 2023-06-21T03:35:58Z
updated_at: 2023-06-21T03:58:21Z
url: https://github.com/astral-sh/ruff/issues/5239
synced_at: 2026-01-12T15:54:45Z
```

# [BUG] Ruff crashed when try run in django project with poetry

---

_@moreiralucas_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
This is my command with result:

```sh
RUST_BACKTRACE=full ruff booking/utils/brevo_client.py
```
The output of command:
```sh
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff_cli/src/commands/run.rs:112:65
stack backtrace:
   0:     0x55dd2ec5287d - <unknown>
   1:     0x55dd2e527e3f - <unknown>
   2:     0x55dd2ec22f44 - <unknown>
   3:     0x55dd2ec5484f - <unknown>
   4:     0x55dd2ec5441f - <unknown>
   5:     0x55dd2ec5413d - <unknown>
   6:     0x55dd2eaba737 - <unknown>
   7:     0x55dd2ec55230 - <unknown>
   8:     0x55dd2ec54f74 - <unknown>
   9:     0x55dd2ec54f06 - <unknown>
  10:     0x55dd2ec54ef1 - <unknown>
  11:     0x55dd2e421702 - <unknown>
  12:     0x55dd2e4217fc - <unknown>
  13:     0x55dd2eaa11c1 - <unknown>
  14:     0x55dd2ea84695 - <unknown>
  15:     0x55dd2ea51f66 - <unknown>
  16:     0x55dd2ea4846f - <unknown>
  17:     0x55dd2e4706f7 - <unknown>
  18:     0x55dd2e46c073 - <unknown>
  19:     0x55dd2e47272b - <unknown>
  20:     0x7f4bb7e29d90 - __libc_start_call_main
                               at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  21:     0x7f4bb7e29e40 - __libc_start_main_impl
                               at ./csu/../csu/libc-start.c:392:3
  22:     0x55dd2e4694d9 - <unknown>
  23:                0x0 - <unknown>
  ```

pyproject.toml:
```toml
[tool.poetry.group.dev.dependencies]
ruff = "0.0.273"
black = "^23.3.0"
model-bakery = "^1.12.0"
isort = "^5.12.0"
pylint = "^2.17.4"
pylint-django = "^2.5.3"
djlint = "^1.31.0"

[tool.ruff]
select = ["RUF", "I", "PL", "F", "COM", "UP", "DJ", "T10", "T20", "DTZ", "SIM", "TID", "PTH", "ERA", "TRY"]
ignore = ["RUF012", "PLR2004", "PLR0911", "PLR0913", "PLR0915", "DJ001", "DJ001", "DJ008", "TRY003"]
line-length = 120
# Assume Python 3.11.
target-version = "py311"

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
"settings.py" = ["F403", "F404", "F405"]

[tool.ruff.isort]
known-first-party = ["luxuryvacation", "booking"]

[tool.isort]
profile = "black"
multi_line_output = 3
include_trailing_comma = true
force_grid_wrap = 0
use_parentheses = true
line_length = 120

[tool.djlint]
max_line_length=120

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

---

_Comment by @charliermarsh on 2023-06-21 03:47_

Hey sorry -- a new release is building now that resolves this.

---

_Comment by @charliermarsh on 2023-06-21 03:58_

Should be fixed in v0.0.274, which is out now.

---

_Closed by @charliermarsh on 2023-06-21 03:58_

---

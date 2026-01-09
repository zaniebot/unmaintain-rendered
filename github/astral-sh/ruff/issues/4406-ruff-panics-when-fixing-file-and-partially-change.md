---
number: 4406
title: Ruff panics when fixing file and partially change its content
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-05-12T21:59:45Z
updated_at: 2023-10-01T08:19:08Z
url: https://github.com/astral-sh/ruff/issues/4406
synced_at: 2026-01-07T13:12:14-06:00
---

# Ruff panics when fixing file and partially change its content

---

_Issue opened by @qarmin on 2023-05-12 21:59_

Ruff f5be3d8e5b2e3f3a0c5890075b552371f4061023 - still happens 0.0.285

Command - `ruff --fix` with config with all rules enabled
```
"""
    ~~~~~~~~~~~~~~~~~~~~
```
cause to show error 
```
error: warning: Linting panicked PY_FILE_TEST_2372106092.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'byte index 31 is out of bounds of `"""
    ~~~~~~~~~~~~~~~~~~~~`', crates/ruff_python_ast/src/source_code/line_index.rs:113:21
...
   6: core::panicking::panic_fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panicking.rs:64:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/str/mod.rs:86:9
   9: ruff_python_ast::source_code::line_index::LineIndex::source_location
  10: <ruff::logging::DisplayParseError as core::fmt::Display>::fmt
  11: core::fmt::write
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:1232:17
  12: <core::fmt::Arguments as core::fmt::Display>::fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:559:9
  13: <&T as core::fmt::Display>::fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:2396:62
  14: core::fmt::write
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:1232:17
  15: <core::fmt::Arguments as core::fmt::Display>::fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:559:9
  16: <&T as core::fmt::Display>::fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:2396:62
  17: core::fmt::write
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/fmt/mod.rs:1232:17
  18: <fern::log_impl::Output as log::Log>::log
  19: fern::log_impl::FormatCallback::finish
  20: ruff::logging::set_up_logging::{{closure}}
  21: <fern::log_impl::Dispatch as log::Log>::log
  22: ruff_cli::diagnostics::lint_path
  23: rayon::iter::plumbing::bridge_producer_consumer::helper
  24: ruff_cli::commands::run::run
  25: ruff_cli::run
  26: ruff::main
 ....

```

File before and after fixing(1 byte of difference) -  [tt.zip](https://github.com/charliermarsh/ruff/files/11468011/tt.zip)


---

_Label `bug` added by @charliermarsh on 2023-05-12 22:03_

---

_Comment by @qarmin on 2023-05-13 19:16_

Not 100% sure, but looks that this may be caused by https://github.com/charliermarsh/ruff/pull/3931

---

_Comment by @zanieb on 2023-05-14 05:30_

When the file is modified subsequent calls do not panic, you can reproduce with `--diff` as well. A bisect confirms this was introduced by #3931.

You can narrow selection to `--select W292,E999`. This appears to be a panic while displaying an EOF parse error (E999) with an out of bounds offset after a newline added to the end of the file (per W292). 



---

_Comment by @qarmin on 2023-10-01 08:19_

Files now throws parse error(same as cpython)

---

_Closed by @qarmin on 2023-10-01 08:19_

---

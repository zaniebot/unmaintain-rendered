---
number: 11449
title: Panic when quoting annotations
type: issue
state: closed
author: thejcannon
labels:
  - bug
assignees: []
created_at: 2024-05-16T14:34:11Z
updated_at: 2024-05-16T16:13:10Z
url: https://github.com/astral-sh/ruff/issues/11449
synced_at: 2026-01-07T13:12:15-06:00
---

# Panic when quoting annotations

---

_Issue opened by @thejcannon on 2024-05-16 14:34_

Version: `0.4.4`
Command: `ruff check --fix --unsafe-fixes foo.py`

I whittled my repro down to:

```python
from .t import RO, RP
from .urs import UR


class MUR(UR):
    def cr(self) -> RO | list[RP]:
        pass

```

My config is:
```toml
[lint]
select = ["TCH"]

[lint.flake8-type-checking]
quote-annotations = true
```

Stack is:
```
panicked at /Users/runner/work/ruff/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: std::backtrace::Backtrace::force_capture
   1: <ruff::panic::PanicError as core::fmt::Display>::fmt
   2: std::panicking::rust_panic_with_hook
   3: <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box
   4: <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt
   5: _rust_begin_unwind
   6: core::panicking::panic_fmt
   7: core::panicking::panic
   8: ruff_linter::rules::tryceratops::rules::verbose_raise::<impl core::convert::From<ruff_linter::rules::tryceratops::rules::verbose_raise::VerboseRaise> for ruff_diagnostics::diagnostic::DiagnosticKind>::from
   9: ruff_linter::linter::lint_fix
  10: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  11: ruff::resolve::resolve
  12: <ruff::panic::PanicError as core::fmt::Display>::fmt
  13: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  14: <ruff::args::LogLevelArgs as clap_builder::derive::Args>::augment_args
  15: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  16: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  17: ruff::check
  18: ruff::run
  19: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  20: _main
  21: _main
  22: std::rt::lang_start_internal
  23: _main
```

---

_Comment by @trag1c on 2024-05-16 14:38_

I can't seem to reproduce this on v0.4.4 🤔 

---

_Comment by @thejcannon on 2024-05-16 14:39_

I'm on `0.4.3`, let me try `0.4.4`
(Meant to put that in my OP)

---

_Comment by @thejcannon on 2024-05-16 14:42_

Yup! Works in `0.4.4`. Nothing in [the changelog](https://github.com/astral-sh/ruff/releases/tag/v0.4.4) sticks out to me, but oh well.

---

_Closed by @thejcannon on 2024-05-16 14:42_

---

_Reopened by @thejcannon on 2024-05-16 14:51_

---

_Comment by @thejcannon on 2024-05-16 14:51_

Whoops, spoke to soon. Got it on `0.4.4`, I forgot to include CLI command.

`ruff check --fix --unsafe-fixes`

---

_Comment by @trag1c on 2024-05-16 14:55_

Yup, can reproduce now:
```
error: Panicked while linting foo.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /Users/runner/work/ruff/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: std::backtrace::Backtrace::force_capture
   1: <ruff::panic::PanicError as core::fmt::Display>::fmt
   2: std::panicking::rust_panic_with_hook
   3: <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box
   4: <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt
   5: _rust_begin_unwind
   6: core::panicking::panic_fmt
   7: core::panicking::panic
   8: ruff_linter::rules::tryceratops::rules::verbose_raise::<impl core::convert::From<ruff_linter::rules::tryceratops::rules::verbose_raise::VerboseRaise> for ruff_diagnostics::diagnostic::DiagnosticKind>::from
   9: ruff_linter::linter::lint_fix
  10: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  11: ruff::resolve::resolve
  12: <ruff::panic::PanicError as core::fmt::Display>::fmt
  13: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  14: <ruff::args::LogLevelArgs as clap_builder::derive::Args>::augment_args
  15: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  16: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  17: ruff::check
  18: ruff::run
  19: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  20: _main
  21: _main
  22: std::rt::lang_start_internal
  23: _main


All checks passed!
```

---

_Comment by @charliermarsh on 2024-05-16 14:55_

Thx.

---

_Label `bug` added by @charliermarsh on 2024-05-16 14:55_

---

_Renamed from "[Linter panic] " to "Panic when quoting annotations" by @charliermarsh on 2024-05-16 14:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-16 15:12_

---

_Comment by @charliermarsh on 2024-05-16 15:13_

I see the issue.

---

_Referenced in [astral-sh/ruff#11452](../../astral-sh/ruff/pulls/11452.md) on 2024-05-16 15:59_

---

_Closed by @charliermarsh on 2024-05-16 16:13_

---

_Closed by @charliermarsh on 2024-05-16 16:13_

---

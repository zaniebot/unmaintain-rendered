```yaml
number: 11107
title: Ruff should give a nonzero exit code if file linting panics
type: issue
state: open
author: AlexWaygood
labels:
  - cli
assignees: []
created_at: 2024-04-23T16:17:05Z
updated_at: 2024-04-23T16:27:46Z
url: https://github.com/astral-sh/ruff/issues/11107
synced_at: 2026-01-12T15:54:50Z
```

# Ruff should give a nonzero exit code if file linting panics

---

_@AlexWaygood_

Currently if Ruff panics, you get this on the command line:

```
% ruff check foo.py --no-cache
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
   8: <ruff_python_parser::lexer::LexicalErrorType as core::fmt::Display>::fmt
   9: <ruff_python_parser::lexer::LexicalErrorType as core::fmt::Display>::fmt
  10: <ruff_python_parser::lexer::LexicalErrorType as core::fmt::Display>::fmt
  11: ruff_python_parser::parser::Program::parse_tokens
  12: ruff_python_parser::parser::Program::parse_tokens
  13: ruff_python_parser::parse_tokens
  14: ruff_linter::linter::check_path
  15: ruff_linter::linter::lint_only
  16: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  17: <ruff::panic::PanicError as core::fmt::Display>::fmt
  18: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  19: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  20: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  21: <ruff::diagnostics::FixMap as core::ops::arith::AddAssign>::add_assign
  22: ruff::check
  23: ruff::run
  24: <ruff::cache::PackageCacheMap as ruff::cache::PackageCaches>::get
  25: _main
  26: _main
  27: std::rt::lang_start_internal
  28: _main


All checks passed!
% echo $?
0
```

Pay no attention to the specific cause of the panic here or the contents of `foo.py` (this specific panic has already been fixed on `main`). If Ruff _does_ panic, though:
- We shouldn't really be exiting with code 0 -- it should probably be exit code 2, to indicate that the process crashed with an internal error.
- We also _probably_ shouldn't print the chirpy "All checks passed!" message at the end

---

_Label `cli` added by @AlexWaygood on 2024-04-23 16:17_

---

_Renamed from "Ruff should give a nonzero exit code if it panics" to "Ruff should give a nonzero exit code if lint rule panics" by @MichaReiser on 2024-04-23 16:17_

---

_Renamed from "Ruff should give a nonzero exit code if lint rule panics" to "Ruff should give a nonzero exit code if file linting panics" by @MichaReiser on 2024-04-23 16:18_

---

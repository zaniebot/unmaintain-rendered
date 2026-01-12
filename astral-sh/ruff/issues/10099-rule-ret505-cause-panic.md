```yaml
number: 10099
title: Rule RET505 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-02-23T15:17:11Z
updated_at: 2024-08-14T16:09:12Z
url: https://github.com/astral-sh/ruff/issues/10099
synced_at: 2026-01-12T15:54:49Z
```

# Rule RET505 cause panic

---

_@qarmin_

ruff 0.2.2 (also happens with 0.3.5)
 (latest changes from main branch)
```
ruff  *.py --select RET505 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
class Plotter:
    def field_plot(self, df, label, time=None, res=None, neuron=True, dynamic=True):
            for e in edges:
                    line += [
                        {
                        }
                    ]
            line += [
            ]
            if not dynamic:
                return p_graph + p_line + p_field
            else:
\
                Branch = hv.streams.Stream.define("branch", branch=None)
```

error
```

error: Panicked while linting /opt/tmp_folder/F_NAME_7062544777190882164.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_trivia/src/textwrap.rs:155:22:
attempt to subtract with overflow
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/alloc/src/boxed.rs:2029:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:649:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/panicking.rs:144:5
   8: ruff_python_trivia::textwrap::dedent_to
             at ./ruff/crates/ruff_python_trivia/src/textwrap.rs:155:22
   9: ruff_linter::fix::edits::adjust_indentation
             at ./ruff/crates/ruff_linter/src/fix/edits.rs:212:12
  10: ruff_linter::rules::flake8_return::rules::function::remove_else
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:851:24
  11: ruff_linter::rules::flake8_return::rules::function::superfluous_else_node::{{closure}}
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:641:25
  12: ruff_diagnostics::diagnostic::Diagnostic::try_set_fix
             at ./ruff/crates/ruff_diagnostics/src/diagnostic.rs:57:15
  13: ruff_linter::rules::flake8_return::rules::function::superfluous_else_node
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:640:21
  14: ruff_linter::rules::flake8_return::rules::function::superfluous_elif_else
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:720:9
  15: ruff_linter::rules::flake8_return::rules::function::function
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:757:9
  16: ruff_linter::checkers::ast::analyze::statement::statement
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/statement.rs:227:17
  17: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:918:9
  18: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1524:13
  19: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:730:17
  20: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1524:13
  21: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2189:5
  22: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:156:40
  23: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:447:18
  24: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:323:22
  25: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  26: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  27: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  28: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  29: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  30: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  31: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  32: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:123:36
  33: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:179:20
  34: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:110:9
  35: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:438:13
  36: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:397:12
  37: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:373:13
  38: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:732:9
  39: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:357:12
  40: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:708:9
  41: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:46:9
  42: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/fold.rs:59:9
  43: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/reduce.rs:15:5
  44: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/mod.rs:991:9
  45: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  46: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:419:13
  47: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:201:33
  48: ruff::main
             at ./ruff/crates/ruff/src/main.rs:49:11
  49: core::ops::function::FnOnce::call_once
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/ops/function.rs:250:5
  50: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/sys_common/backtrace.rs:155:18
  51: std::rt::lang_start::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:166:18
  52: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/ops/function.rs:284:13
  53: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  54: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  55: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  56: std::rt::lang_start_internal::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:148:48
  57: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  58: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  59: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  60: std::rt::lang_start_internal
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:148:20
  61: main
  62: <unknown>
  63: __libc_start_main
  64: _start

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14386820/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2024-02-23 15:18_

---

_Label `help wanted` added by @MichaReiser on 2024-02-23 15:18_

---

_Comment by @mikeleppane on 2024-02-24 13:40_

👍 That hard line break causes that panic situation. Haven't had time to dig deeper into what causes this. 
black formatted is not able to parse the file. However ruff formatter is able to handle this and after the formatting, it will not panic.   

I'm getting the following when trying to fix the file (ruff cli):
```bash
panicked at crates/ruff_python_trivia/src/textwrap.rs:172:52:
byte index 18446744073709551604 is out of bounds of `\
```
   

---

_Comment by @mikeleppane on 2024-02-24 19:08_

Update:
The problem is, as indicated in the backtrace:

> panicked at crates/ruff_python_trivia/src/textwrap.rs:155:22:
attempt to subtract with overflow

that we try to unsafe subtraction, and therefore Rust panics with overflow error. However, the calling function (dedent_to, textwrap.rs) indicates that it panics in the following case:
```rust
/// # Panics
/// If the first line is indented by less than the provided indent.
pub fn dedent_to(text: &str, indent: &str) -> String {
```
Instead of panicking, we should cleanly shut down with a proper error message. 

The problem occurs on the following line (155) in `ruff/crates/ruff_python_trivia/src/textwrap.rs`:
```rust 
let dedent_len = existing_indent_len - indent.len();
```

Here some debug prints about the used variables:

```bash
INPUT TEXT: "\\\n                Branch = hv.streams.Stream.define(\"branch\", branch=None)"
INDENT: "            "
EXISTING INDENT LEN: 0
INDENT LEN: 12
```





---

_Comment by @qarmin on 2024-05-31 14:38_

Still panics with ruff 0.4.6

file content:
```
def has_untracted_files():
    if b'Untracked files' in result.stdout:
        return True
    else:
\
        return False
```

error
```
All checks passed!

error: Panicked while linting /home/rafal/test/tmp_folder/284806655513326825.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_trivia/src/textwrap.rs:172:52:
byte index 18446744073709551612 is out of bounds of `\
`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
   1: std::panicking::rust_panic_with_hook
   2: std::panicking::begin_panic_handler::{{closure}}
   3: std::sys_common::backtrace::__rust_end_short_backtrace
   4: rust_begin_unwind
   5: core::panicking::panic_fmt
   6: core::str::slice_error_fail_rt
   7: core::str::slice_error_fail
   8: ruff_python_trivia::textwrap::dedent_to
   9: ruff_linter::fix::edits::adjust_indentation
  10: ruff_linter::rules::flake8_return::rules::function::remove_else
  11: ruff_linter::checkers::ast::analyze::statement::statement
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
  13: ruff_linter::checkers::ast::check_ast
  14: ruff_linter::linter::check_path
  15: ruff_linter::linter::lint_fix
  16: ruff::diagnostics::lint_path
  17: ruff::commands::check::lint_path
  18: rayon::iter::plumbing::Producer::fold_with
  19: rayon::iter::plumbing::bridge_producer_consumer::helper
  20: ruff::commands::check::check
  21: ruff::check
  22: ruff::run
  23: ruff::main
  24: std::sys_common::backtrace::__rust_begin_short_backtrace
  25: std::rt::lang_start::{{closure}}
  26: std::rt::lang_start_internal
  27: main
  28: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  29: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  30: _start

```

[python_compressed.zip](https://github.com/user-attachments/files/15515242/python_compressed.zip)


---

_Closed by @AlexWaygood on 2024-08-14 16:09_

---

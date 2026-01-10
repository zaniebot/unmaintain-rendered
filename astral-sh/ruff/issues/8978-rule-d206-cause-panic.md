```yaml
number: 8978
title: Rule D206 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-12-03T08:29:28Z
updated_at: 2023-12-03T20:45:31Z
url: https://github.com/astral-sh/ruff/issues/8978
synced_at: 2026-01-10T11:09:51Z
```

# Rule D206 cause panic

---

_Issue opened by @qarmin on 2023-12-03 08:29_



Ruff 0.1.6 (latest changes from main branch)
```
ruff  *.py --select D206 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class CustomLogger(logging.Logger):
            print("{}-- function : {}-- rum time : {}ms ".format(title, func.__name__, RUN_TIME(T1 - T0)))
def memory_test():
    """
　　　参数含义：precision:精确到小数点后几位
    """
```
Similar problem also happens with D207
```
class CustomLogger(logging.Logger):
    def __init__(self, filename, when='h', interval=1, backupCount=0, encoding=None, delay=False, utc=False,
                 atTime=None):
                raise ValueError("You must specify a day for weekly rollover from 0 to 6 (0 is Monday): %s" % self.when)
def memory_test():
    """
　　　stream:此模块分析结果保存到 'memory_profiler.log' 日志文件。如果没有此参数，分析结果会在控制台输出
    """
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_4464109352984095306.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs:225:21:
attempt to subtract with overflow
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:735:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:601:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:597:5
   6: core::panicking::panic_fmt
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:127:5
   8: ruff_linter::rules::pydocstyle::rules::indent::indent
             at /home/rafal/test/ruff/crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs:225:21
   9: ruff_linter::checkers::ast::analyze::definitions::definitions
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/analyze/definitions.rs:224:17
  10: ruff_linter::checkers::ast::check_ast
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2035:5
  11: ruff_linter::linter::check_path
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:156:40
  12: ruff_linter::linter::lint_fix
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:487:22
  13: ruff_cli::diagnostics::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/diagnostics.rs:281:14
  14: ruff_cli::commands::check::lint_path::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:199:9
  15: std::panicking::try::do_call
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:504:40
  16: std::panicking::try
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:468:19
  17: std::panic::catch_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panic.rs:142:14
  18: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  19: ruff_cli::commands::check::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:198:18
  20: ruff_cli::commands::check::check::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:99:17
  21: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  22: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  23: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  24: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  25: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  26: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  27: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:732:9
  28: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  29: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:708:9
  30: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  31: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/fold.rs:59:9
  32: rayon::iter::reduce::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/reduce.rs:15:5
  33: rayon::iter::ParallelIterator::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:991:9
  34: ruff_cli::commands::check::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:170:10
  35: ruff_cli::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:405:13
  36: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:201:33
  37: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  38: core::ops::function::FnOnce::call_once
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/ops/function.rs:250:5
  39: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/sys_common/backtrace.rs:154:18
  40: main
  41: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  42: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  43: _start


```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13538399/python_compressed.zip)


---

_Comment by @FishAlchemist on 2023-12-03 14:56_

crates\ruff_linter\src\rules\pydocstyle\rules\indent.rs:225
```rust
 line_indent.chars().count() - docstring.indentation.chars().count()
```
line_indent.chars() = Chars(['\u{3000}', '\u{3000}', '\u{3000}'])
count = 3
Unicode+3000 => IDEOGRAPHIC SPACE

docstring.indentation.chars() = Chars([' ', ' ', ' ', ' ']) 
count = 4

---

_Label `bug` added by @charliermarsh on 2023-12-03 20:39_

---

_Label `fuzzer` added by @charliermarsh on 2023-12-03 20:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-03 20:39_

---

_Closed by @charliermarsh on 2023-12-03 20:45_

---

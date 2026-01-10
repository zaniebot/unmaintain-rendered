---
number: 5261
title: "[Panic] `check` and `--fix` crash when called on a nested file next to an `__init__.py`"
type: issue
state: closed
author: Amar1729
labels: []
assignees: []
created_at: 2023-06-21T17:59:11Z
updated_at: 2023-06-21T21:14:43Z
url: https://github.com/astral-sh/ruff/issues/5261
synced_at: 2026-01-10T01:22:44Z
---

# [Panic] `check` and `--fix` crash when called on a nested file next to an `__init__.py`

---

_Issue opened by @Amar1729 on 2023-06-21 17:59_

### Minimal file structure

```
tmp/ruff-dbg $ tree -a

.
├── rest_api
│   ├── __init__.py
│   └── views
│       ├── __init__.py
│       └── tag.py
└── ruff.toml

3 directories, 4 files
```

Both of the `__init__.py`s are empty.

```toml
# ruff.toml

exclude = [
    ".git",
    ".tox",
    "dist",
]
ignore = ["E501"]
select = [
    "E",
    "F",
    "W",
]
```

```python
# tag.py
class Binary:
    authentication = None
```

`tag.py` has some error/warning (i tested with `W291` trailing whitespace after `None` and `W292` no newline at end of file).

### Errors

Calling `ruff check <file>` or `ruff --fix <file>` crashes ruff; calling `ruff check .` or `ruff --fix .` works as expected. Happens with `--isolated` as well.

```
$ RUST_BACKTRACE=full ruff check rest_api/views/tag.py


error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff_cli/src/commands/run.rs:112:65
stack backtrace:
   0:        0x1012eaa40 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hf56058dac04a8100
   1:        0x100d478a0 - core::fmt::write::h887ee594c50d2f6b
   2:        0x10130028c - std::io::Write::write_fmt::h8e068fbe7ba944b7
   3:        0x1012ea848 - std::sys_common::backtrace::print::hefee1a8be582057a
   4:        0x1012fbe84 - std::panicking::default_hook::{{closure}}::h7ba479390a999ae5
   5:        0x1012fbb60 - std::panicking::default_hook::hc4ff20421fd3aa8b
   6:        0x1011b4a9c - ruff_cli::run::{{closure}}::h263f8a459341438f
   7:        0x1012fc774 - std::panicking::rust_panic_with_hook::h97e5266e8ce2f24f
   8:        0x1012ead84 - std::panicking::begin_panic_handler::{{closure}}::h5c38d4c71a65b53e
   9:        0x1012ead18 - std::sys_common::backtrace::__rust_end_short_backtrace::h9c79ccffe3575672
  10:        0x1012fc2c0 - _rust_begin_unwind
  11:        0x10137f754 - core::panicking::panic_fmt::h8d86c61b68da2636
  12:        0x10137f7dc - core::panicking::panic::h6860fe84587621e0
  13:        0x10119e1cc - rayon::iter::plumbing::bridge_producer_consumer::helper::h3304d33f31889e31
  14:        0x101185a60 - ruff_cli::commands::run::run::h555b6a056bc5d1dd
  15:        0x101163470 - ruff_cli::check::hef3a5701c3b81251
  16:        0x10115b754 - ruff_cli::run::h1bb63e1856dc6568
  17:        0x100cc70e4 - ruff::main::h0a10441f674e37c2
  18:        0x100cc3978 - std::sys_common::backtrace::__rust_begin_short_backtrace::h96b0964bb5bd6d72
  19:        0x100cc83e8 - _main

$ RUST_BACKTRACE=full ruff --fix rest_api/views/tag.py


error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff_cli/src/commands/run.rs:112:65
stack backtrace:
   0:        0x1026faa40 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hf56058dac04a8100
   1:        0x1021578a0 - core::fmt::write::h887ee594c50d2f6b
   2:        0x10271028c - std::io::Write::write_fmt::h8e068fbe7ba944b7
   3:        0x1026fa848 - std::sys_common::backtrace::print::hefee1a8be582057a
   4:        0x10270be84 - std::panicking::default_hook::{{closure}}::h7ba479390a999ae5
   5:        0x10270bb60 - std::panicking::default_hook::hc4ff20421fd3aa8b
   6:        0x1025c4a9c - ruff_cli::run::{{closure}}::h263f8a459341438f
   7:        0x10270c774 - std::panicking::rust_panic_with_hook::h97e5266e8ce2f24f
   8:        0x1026fad84 - std::panicking::begin_panic_handler::{{closure}}::h5c38d4c71a65b53e
   9:        0x1026fad18 - std::sys_common::backtrace::__rust_end_short_backtrace::h9c79ccffe3575672
  10:        0x10270c2c0 - _rust_begin_unwind
  11:        0x10278f754 - core::panicking::panic_fmt::h8d86c61b68da2636
  12:        0x10278f7dc - core::panicking::panic::h6860fe84587621e0
  13:        0x1025ae1cc - rayon::iter::plumbing::bridge_producer_consumer::helper::h3304d33f31889e31
  14:        0x102595a60 - ruff_cli::commands::run::run::h555b6a056bc5d1dd
  15:        0x102573470 - ruff_cli::check::hef3a5701c3b81251
  16:        0x10256b754 - ruff_cli::run::h1bb63e1856dc6568
  17:        0x1020d70e4 - ruff::main::h0a10441f674e37c2
  18:        0x1020d3978 - std::sys_common::backtrace::__rust_begin_short_backtrace::h96b0964bb5bd6d72
  19:        0x1020d83e8 - _main

$  RUST_BACKTRACE=full ruff --isolated check rest_api/views/tag.py


error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>error' panicked at 'called `Option::unwrap()` on a `None` value', : Failed to lint check: crates/ruff_cli/src/commands/run.rsNo such file or directory (os error 2):
112:65
stack backtrace:
   0:        0x10521ea40 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hf56058dac04a8100
   1:        0x104c7b8a0 - core::fmt::write::h887ee594c50d2f6b
   2:        0x10523428c - std::io::Write::write_fmt::h8e068fbe7ba944b7
   3:        0x10521e848 - std::sys_common::backtrace::print::hefee1a8be582057a
   4:        0x10522fe84 - std::panicking::default_hook::{{closure}}::h7ba479390a999ae5
   5:        0x10522fb60 - std::panicking::default_hook::hc4ff20421fd3aa8b
   6:        0x1050e8a9c - ruff_cli::run::{{closure}}::h263f8a459341438f
   7:        0x105230774 - std::panicking::rust_panic_with_hook::h97e5266e8ce2f24f
   8:        0x10521ed84 - std::panicking::begin_panic_handler::{{closure}}::h5c38d4c71a65b53e
   9:        0x10521ed18 - std::sys_common::backtrace::__rust_end_short_backtrace::h9c79ccffe3575672
  10:        0x1052302c0 - _rust_begin_unwind
  11:        0x1052b3754 - core::panicking::panic_fmt::h8d86c61b68da2636
  12:        0x1052b37dc - core::panicking::panic::h6860fe84587621e0
  13:        0x1050d21cc - rayon::iter::plumbing::bridge_producer_consumer::helper::h3304d33f31889e31
  14:        0x1050dd0a8 - <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute::h133bec64b0c41269
  15:        0x1052bb1e0 - rayon_core::registry::WorkerThread::wait_until_cold::hf26be6610b8e2b07
  16:        0x104d98584 - std::sys_common::backtrace::__rust_begin_short_backtrace::ha3ea6fda9d4e1206
  17:        0x104d9838c - core::ops::function::FnOnce::call_once{{vtable.shim}}::hfe1f6dc4c31547b1
  18:        0x10521f52c - std::sys::unix::thread::Thread::new::thread_start::h26d49c46532d1c2b
  19:        0x183b2bfa8 - __pthread_joiner_wake

$ ruff check .

rest_api/views/tag.py:2:26: W291 [*] Trailing whitespace
Found 1 error.
[*] 1 potentially fixable with the --fix option.

$ ruff --fix .

Found 1 error (1 fixed, 0 remaining).
```

### Versions

- `ruff --version`: `ruff 0.0.273`
  - installed by homebrew
- as a note this project is using `python3.8` (but there are no version-specific files, such as `.python-version`, in the minimal example)
- OS: macOS (m1) Ventura 13.4
- architecture `uname -m`: `arm64`

### Fixes (hacks?)

I can "fix" this locally by removing **either or both** of the `__init__.py` files (under `rest_api` or `views`). Leaving both of them results in the above failure. Panic still occurs if i cd into `rest_api/views` and call `ruff check tag.py`. Tested both with and without `.ruff_cache/`

---

_Comment by @dhruvmanila on 2023-06-21 18:11_

This is fixed on the latest version (`v0.0.274`).

---

_Closed by @charliermarsh on 2023-06-21 18:25_

---

_Comment by @Amar1729 on 2023-06-21 21:13_

My bad, thanks! Now I'm just extremely confused why `brew outdated` didn't tell me that ruff was outdated (but `brew uninstall ruff && brew install ruff` did) 🤷 

---

_Comment by @charliermarsh on 2023-06-21 21:14_

No worries! Maybe it wasn't up yet, I'm not sure!

---

```yaml
number: 3385
title: "[Panic] index out of bounds"
type: issue
state: closed
author: Radiergummi
labels:
  - bug
assignees: []
created_at: 2023-03-07T13:45:02Z
updated_at: 2023-03-08T20:39:01Z
url: https://github.com/astral-sh/ruff/issues/3385
synced_at: 2026-01-10T11:09:46Z
```

# [Panic] index out of bounds

---

_Issue opened by @Radiergummi on 2023-03-07 13:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I just experienced a reproducible crash. This seems to be caused by the combination of `+D` and `-D212`; it also prints the following warning: `'one-blank-line-before-class' (D203) and 'no-blank-line-before-class' (D211) are incompatible. Ignoring 'one-blank-line-before-class'.`

- Command: `ruff --select D --ignore D212 src --fix`
- Backtrace:
  ```rust
  thread '<unnamed>' panicked at 'index out of bounds: the len is 29 but the index is 29', crates/ruff/src/source_code/locator.rs:83:9
  stack backtrace:
     0:        0x10d897cfa - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h521420ec33f3769d
     1:        0x10d2cc20a - core::fmt::write::h694a0d7c23f57ada
     2:        0x10d894a5c - std::io::stdio::_eprint::hce138c1c2856ce51
     3:        0x10d897aca - std::time::Instant::elapsed::h513af9279b3b413c
     4:        0x10d89bf93 - std::panicking::default_hook::hfeeab2c667b2d7c2
     5:        0x10d89bcf7 - std::panicking::default_hook::hfeeab2c667b2d7c2
     6:        0x10d526270 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
     7:        0x10d89c697 - std::panicking::rust_panic_with_hook::h1b5245192f90251d
     8:        0x10d89c435 - <std::panicking::begin_panic_handler::StrPanicPayload as core::panic::BoxMeUp>::get::h9f4a61dc8b1a8670
     9:        0x10d89a8e8 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h521420ec33f3769d
    10:        0x10d89c142 - _rust_begin_unwind
    11:        0x10d922713 - core::panicking::panic_fmt::h0097ad8ec0b07517
    12:        0x10d922856 - core::panicking::panic_bounds_check::h1c89633b3be15e5a
    13:        0x10d7036e8 - <ruff::source_code::indexer::Indexer as core::convert::From<&[core::result::Result<(rustpython_compiler_core::location::Location,rustpython_parser::token::Tok,rustpython_compiler_core::location::Location),rustpython_parser::lexer::LexicalError>]>>::from::h495512723e7b0d12
    14:        0x10d703ce8 - ruff::source_code::locator::Locator::slice::h8cda49452a10e3a1
    15:        0x10d64e55c - ruff::linter::lint_fix::h4a770bbd14b0f19a
    16:        0x10d51025a - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    17:        0x10d49ffaa - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    18:        0x10d457931 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    19:        0x10d4a0d67 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    20:        0x10d457a75 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    21:        0x10d4a0d67 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    22:        0x10d457931 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    23:        0x10d4a0d67 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    24:        0x10d457931 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    25:        0x10d4a0d67 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    26:        0x10d4cff25 - <regex_syntax::hir::HirKind as core::fmt::Debug>::fmt::ha120f21aa78097c1
    27:        0x10d92b35a - rayon_core::registry::WorkerThread::wait_until_cold::h38f60dff60493700
    28:        0x10d3f0df7 - rayon_core::registry::ThreadBuilder::run::hd4e0e31ddec10142
    29:        0x10d3ee002 - <quick_xml::events::attributes::AttrError as core::fmt::Debug>::fmt::ha945229a9a8fc49b
    30:        0x10d3ef36e - <quick_xml::events::attributes::AttrError as core::fmt::Debug>::fmt::ha945229a9a8fc49b
    31:        0x10d8a0319 - std::sys::unix::thread::Thread::new::h135242531a117d11
    32:     0x7ff813b49259 - __pthread_start
  ```
- `ruff.toml`:
  ```toml
  select = [
      "E",
      "F",
      "I002",
      "N",
      "UP",
      "PTH",
      "COM",
      "RET",
      "T",
      "S",
      "INP",
      "TCH",
      "SIM",
      "G",
      "ISC",
      "TRY",
      "DJ",
      "ARG",
      "PLW",
      "DTZ",
      "EM",
      "SLF",
  ]
  
  ignore = [
  
      # S101: Assert usage
      # Not using assert is just bogus advice.
      "S101",
  
      # S104: Possible binding to all interfaces
      # We're running in Docker, where binding to all interfaces is explicitly
      # desired behaviour.
      "S104",
  
      # S324: Probable use of insecure hash functions in `hashlib`: `sha1`
      # We don't use SHA1 for confidentiality purposes, so this is okay.
      "S324",
  
      # PLW2901: `for` loop variable `<variable>` overwritten by assignment target
      # When applied judiciously this can make sense.
      "PLW2901",
  
      # TRY003: Avoid specifying long messages outside the exception class
      # Effectively, this suggests to create custom exception classes for every
      # possible error. This makes sense, but doesn't add much value now.
      # TODO: Revisit this decision
      "TRY003",
  
      # D100-D104: Existence of doc comments for public code
      # Again, this makes sense, but doesn't add much value now.
      # TODO: Revisit this decision
      "D100",
      "D101",
      "D102",
      "D103",
      "D104",
  
      # D212: Multi-line docstring summary should start at the first line
      # This just makes the comment harder to read.
      "D212",
  
      # DJ001: Avoid using `null=True` on string-based fields such as CharField
      # We have a reason to make a distinction between "" and NULL, so this is OK.
      "DJ001",
      "DJ008",
  
      # RUF100: Unused `<directive>` directive (non-enabled: `<rule>`)
      # These are usually signs of progressive enhancements and should not crash
      # the commit or build.
      "RUF100",
  
      # SLF001: Private member accessed: `<member>`
      # As we have properties in the index prefixed by a dash, we have to use
      # those names in attrs accessors. Hence, this is not a bug for 99% of cases.
      "SLF001",
  ]
  
  [per-file-ignores]
  # Imports are organized differently in the settings modules. This is important
  # and should not be changed.
  "src/sia/settings/__init__.py" = ["E402", "F403"]
  "src/index/models.py" = ["N815"]
  "src/manage.py" = ["INP001"]
  "src/processing/management/commands/*.py" = ["TRY003", "EM101", "EM102"]
  "src/ai/*.py" = ["T201"]
  "bin/hash_rabbitmq_password.py" = ["INP001"]
  ```

---

_Label `bug` added by @charliermarsh on 2023-03-07 14:36_

---

_Comment by @charliermarsh on 2023-03-07 14:37_

Thanks so much! Do you happen to know which file is triggering this? Are you able to include a snippet to reproduce it (or a link to the repo)? 

---

_Comment by @Radiergummi on 2023-03-07 14:41_

Unfortunately no, I accidentally ran black after this and cannot reproduce the bug anymore :( Feel free to close the issue if this isn't helpful without a repro...

---

_Comment by @charliermarsh on 2023-03-08 20:39_

Blerg ok. I'll close for now, but please ping back here if you see this again, I'd love to help!

---

_Closed by @charliermarsh on 2023-03-08 20:39_

---

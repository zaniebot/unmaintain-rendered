```yaml
number: 6856
title: Add information to fuzzer README how correctly report problems found by this tool
type: issue
state: closed
author: qarmin
labels:
  - documentation
  - fuzzer
assignees: []
created_at: 2023-08-24T17:29:53Z
updated_at: 2024-01-08T08:27:14Z
url: https://github.com/astral-sh/ruff/issues/6856
synced_at: 2026-01-12T15:54:46Z
```

# Add information to fuzzer README how correctly report problems found by this tool

---

_@qarmin_

After downloading fuzz corpus, I run command `cargo fuzz run -s none ruff_fix_validity -- -timeout=200` and file with invalid content was created:
```
from twr import (_fter,
  noqa_l)
```
which panics with message(I see this in fuzzer output)
```
thread '<unnamed>' panicked at 'attempt to subtract with overflow', /home/rafal/test/ruff/crates/ruff/src/noqa.rs:67:13
stack backtrace:
   0: rust_begin_unwind
             at /rustc/8ede3aae28fe6e4d52b38157d7bfe0d3bceef225/library/std/src/panicking.rs:593:5
   1: core::panicking::panic_fmt
             at /rustc/8ede3aae28fe6e4d52b38157d7bfe0d3bceef225/library/core/src/panicking.rs:67:14
   2: core::panicking::panic
             at /rustc/8ede3aae28fe6e4d52b38157d7bfe0d3bceef225/library/core/src/panicking.rs:117:5
   3: ruff::noqa::Directive::try_extract
             at /home/rafal/test/ruff/crates/ruff/src/noqa.rs:67:13
   4: ruff::noqa::rule_is_ignored
             at /home/rafal/test/ruff/crates/ruff/src/noqa.rs:217:11
   5: ruff::checkers::ast::Checker::rule_is_ignored
             at /home/rafal/test/ruff/crates/ruff/src/checkers/ast/mod.rs:163:9
   6: ruff::rules::pyflakes::rules::unused_import::unused_import
             at /home/rafal/test/ruff/crates/ruff/src/rules/pyflakes/rules/unused_import.rs:131:12
   7: ruff::checkers::ast::analyze::deferred_scopes::deferred_scopes
             at /home/rafal/test/ruff/crates/ruff/src/checkers/ast/analyze/deferred_scopes.rs:303:17
   8: ruff::checkers::ast::check_ast
             at /home/rafal/test/ruff/crates/ruff/src/checkers/ast/mod.rs:1999:5
   9: ruff::linter::check_path
             at /home/rafal/test/ruff/crates/ruff/src/linter.rs:154:40
  10: ruff::test::test_contents
             at /home/rafal/test/ruff/crates/ruff/src/test.rs:135:9
  11: ruff::test::test_snippet
             at /home/rafal/test/ruff/crates/ruff/src/test.rs:98:5
  12: ruff_fix_validity::do_fuzz
             at ./fuzz_targets/ruff_fix_validity.rs:22:13
  13: rust_fuzzer_test_input
             at /home/rafal/.cargo/git/checkouts/libfuzzer-14de4ab4c067ed36/c9c43f3/src/lib.rs:297:60
  14: libfuzzer_sys::test_input_wrap::{{closure}}
             at /home/rafal/.cargo/git/checkouts/libfuzzer-14de4ab4c067ed36/c9c43f3/src/lib.rs:61:9
  15: std::panicking::try::do_call
             at /rustc/8ede3aae28fe6e4d52b38157d7bfe0d3bceef225/library/std/src/panicking.rs:500:40
  16: std::panicking::try
             at /rustc/8ede3aae28fe6e4d52b38157d7bfe0d3bceef225/library/std/src/panicking.rs:464:19
  17: std::panic::catch_unwind
             at /rustc/8ede3aae28fe6e4d52b38157d7bfe0d3bceef225/library/std/src/panic.rs:142:14

```


I tried to reproduce crash by typing 
```
ruff fuzz/artifacts/ruff_fix_validity/crash-e85787f399673a --fix --select ALL,NURSERY
```
but I got completely normal messages without any crash
```
artifacts/ruff_fix_validity/crash-e85787f399673a898d8551c308c065945de1c21c:1:1: D100 Missing docstring in public module
artifacts/ruff_fix_validity/crash-e85787f399673a898d8551c308c065945de1c21c:1:1: CPY001 Missing copyright notice at top of file
Found 4 errors (2 fixed, 2 remaining).
```

So instruction probably should contains info what to do to properly reproduce problem without fuzzer

https://github.com/astral-sh/ruff/blob/main/fuzz/README.md

---

_Comment by @zanieb on 2023-08-24 19:46_

cc @addisoncrump 

---

_Label `documentation` added by @zanieb on 2023-08-24 19:46_

---

_Label `fuzzer` added by @zanieb on 2023-08-24 19:46_

---

_Comment by @addisoncrump on 2023-08-25 04:48_

You can just specify the file after: `cargo fuzz run -s none ruff_fix_validity <path/to/artifact>`

Currently we use the same test pass/fail as `ruff::test::test_snippet`, which noticeably has some things missing (e.g. we could not reproduce some of the recently opened fuzzer-found issues with `test_snippet`, only with the ruff frontend) and has some things that are not caught by the ruff frontend. We should probably try to unify the test pass/fail conditions with the frontend's. I'll put that in the same PR as the README addition.

---

_Comment by @addisoncrump on 2024-01-03 01:30_

Was this suitably addressed?

---

_Closed by @MichaReiser on 2024-01-08 08:26_

---

_Comment by @MichaReiser on 2024-01-08 08:27_

Let's assume it is. We can re-open it if the question pops up again or create a new issue for it.

---

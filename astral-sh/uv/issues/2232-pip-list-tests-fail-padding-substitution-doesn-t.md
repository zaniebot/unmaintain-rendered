---
number: 2232
title: "pip_list tests fail (padding substitution doesn't work?)"
type: issue
state: closed
author: mgorny
labels: []
assignees: []
created_at: 2024-03-06T07:17:48Z
updated_at: 2024-03-06T13:32:08Z
url: https://github.com/astral-sh/uv/issues/2232
synced_at: 2026-01-10T01:23:14Z
---

# pip_list tests fail (padding substitution doesn't work?)

---

_Issue opened by @mgorny on 2024-03-06 07:17_

When running the test suite, I'm getting 3 test failures from `crates/uv/tests/pip_list.rs`. It seems that for some reason, the "padding substitution" related to `[WORKSPACE_DIR]` change doesn't work for me, though I can't really figure out what's different.

This is Gentoo Linux amd64, uv 0.1.13. Workspace directory is `/var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13`.

```
     Running `/var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/target/debug/deps/pip_list-486514bc49739dd8`

running 5 tests
test empty ... ok^O
test single_no_editable ... ok^O
test editable ... FAILED^O
test exclude ... FAILED^O
test editable_only ... FAILED^O

failures:

---- editable stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: editable-2
Source: crates/uv/tests/pip_list.rs:176
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-Package         Version Editable project location                                
    4       │---------------- ------- ---------------------------------------------------------
    5       │-numpy           1.26.2                                                           
          3 │+Package         Version Editable project location                                                                     
          4 │+--------------- ------- ----------------------------------------------------------------------------------------------
          5 │+numpy           1.26.2                                                                                                
    6     6 │ poetry-editable 0.1.0   [WORKSPACE_DIR]/scripts/editable-installs/poetry_editable
    7     7 │ 
    8     8 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'editable' panicked at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:563:9:
snapshot assertion for 'editable-2' failed in line 176
stack backtrace:
   0:     0x5613b3cf18dc - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1ffcaa4041542985
   1:     0x5613b3d1f1d0 - core::fmt::write::h1b82113e9b693b22
   2:     0x5613b3cee12f - std::io::Write::write_fmt::hd588a8689be4a0fb
   3:     0x5613b3cf16c4 - std::sys_common::backtrace::print::h08a5d46ff202da96
   4:     0x5613b3cf3407 - std::panicking::default_hook::{{closure}}::h0dc7fd1af3fefd78
   5:     0x5613b3cf30f3 - std::panicking::default_hook::hdbcb9fe0d99a03f5
   6:     0x5613b3621217 - test::test_main::{{closure}}::hecd46e023328d5e6
   7:     0x5613b3cf3a18 - std::panicking::rust_panic_with_hook::heb55276fcd948217
   8:     0x5613b3cf376e - std::panicking::begin_panic_handler::{{closure}}::hf186713900a95986
   9:     0x5613b3cf1da6 - std::sys_common::backtrace::__rust_end_short_backtrace::h01811d7a116e9704
  10:     0x5613b3cf34d2 - rust_begin_unwind
  11:     0x5613b3483b25 - core::panicking::panic_fmt::h0827c44294bd0552
  12:     0x5613b34aa03a - insta::runtime::finalize_assertion::ha4af970e8c318200
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:563:9
  13:     0x5613b34aac1b - insta::runtime::assert_snapshot::hb2c3593f1a320f6f
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:680:9
  14:     0x5613b34927a1 - pip_list::editable::h0b18ec13b1138231
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/crates/uv/tests/pip_list.rs:176:5
  15:     0x5613b3490b77 - pip_list::editable::{{closure}}::h1e66b868d582c14d
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/crates/uv/tests/pip_list.rs:97:18
  16:     0x5613b3498f86 - core::ops::function::FnOnce::call_once::h65366f9dd3ee8079
                               at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5
  17:     0x5613b36268df - test::__rust_begin_short_backtrace::h22539721e88f932b
  18:     0x5613b3625885 - test::run_test::{{closure}}::h07bc5892fe0b0b84
  19:     0x5613b35edb46 - std::sys_common::backtrace::__rust_begin_short_backtrace::h94357c6f10d6d2ec
  20:     0x5613b35f2bf7 - core::ops::function::FnOnce::call_once{{vtable.shim}}::h768f50f0af66b68b
  21:     0x5613b3cf9bc5 - std::sys::unix::thread::Thread::new::thread_start::h1abe540572f6a146
  22:     0x7f41c6e824d1 - start_thread
                               at /tmp/portage/sys-libs/glibc-2.39-r2/work/glibc-2.39/nptl/pthread_create.c:447:8
  23:     0x7f41c6effff8 - __GI___clone3
                               at /tmp/portage/sys-libs/glibc-2.39-r2/work/glibc-2.39/misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78
  24:                0x0 - <unknown>

---- exclude stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: exclude-2
Source: crates/uv/tests/pip_list.rs:402
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-Package         Version Editable project location                                
    4       │---------------- ------- ---------------------------------------------------------
          3 │+Package         Version Editable project location                                                                     
          4 │+--------------- ------- ----------------------------------------------------------------------------------------------
    5     5 │ poetry-editable 0.1.0   [WORKSPACE_DIR]/scripts/editable-installs/poetry_editable
    6     6 │ 
    7     7 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'exclude' panicked at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:563:9:
snapshot assertion for 'exclude-2' failed in line 402
stack backtrace:
   0:     0x5613b3cf18dc - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1ffcaa4041542985
   1:     0x5613b3d1f1d0 - core::fmt::write::h1b82113e9b693b22
   2:     0x5613b3cee12f - std::io::Write::write_fmt::hd588a8689be4a0fb
   3:     0x5613b3cf16c4 - std::sys_common::backtrace::print::h08a5d46ff202da96
   4:     0x5613b3cf3407 - std::panicking::default_hook::{{closure}}::h0dc7fd1af3fefd78
   5:     0x5613b3cf30f3 - std::panicking::default_hook::hdbcb9fe0d99a03f5
   6:     0x5613b3621217 - test::test_main::{{closure}}::hecd46e023328d5e6
   7:     0x5613b3cf3a18 - std::panicking::rust_panic_with_hook::heb55276fcd948217
   8:     0x5613b3cf376e - std::panicking::begin_panic_handler::{{closure}}::hf186713900a95986
   9:     0x5613b3cf1da6 - std::sys_common::backtrace::__rust_end_short_backtrace::h01811d7a116e9704
  10:     0x5613b3cf34d2 - rust_begin_unwind
  11:     0x5613b3483b25 - core::panicking::panic_fmt::h0827c44294bd0552
  12:     0x5613b34aa03a - insta::runtime::finalize_assertion::ha4af970e8c318200
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:563:9
  13:     0x5613b34aac1b - insta::runtime::assert_snapshot::hb2c3593f1a320f6f
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:680:9
  14:     0x5613b3497339 - pip_list::exclude::hd9b14f5b7dd1533e
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/crates/uv/tests/pip_list.rs:402:5
  15:     0x5613b3495687 - pip_list::exclude::{{closure}}::h731259c0a00f1f2d
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/crates/uv/tests/pip_list.rs:330:17
  16:     0x5613b3498e86 - core::ops::function::FnOnce::call_once::h12729ce255b33931
                               at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5
  17:     0x5613b36268df - test::__rust_begin_short_backtrace::h22539721e88f932b
  18:     0x5613b3625885 - test::run_test::{{closure}}::h07bc5892fe0b0b84
  19:     0x5613b35edb46 - std::sys_common::backtrace::__rust_begin_short_backtrace::h94357c6f10d6d2ec
  20:     0x5613b35f2bf7 - core::ops::function::FnOnce::call_once{{vtable.shim}}::h768f50f0af66b68b
  21:     0x5613b3cf9bc5 - std::sys::unix::thread::Thread::new::thread_start::h1abe540572f6a146
  22:     0x7f41c6e824d1 - start_thread
                               at /tmp/portage/sys-libs/glibc-2.39-r2/work/glibc-2.39/nptl/pthread_create.c:447:8
  23:     0x7f41c6effff8 - __GI___clone3
                               at /tmp/portage/sys-libs/glibc-2.39-r2/work/glibc-2.39/misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78
  24:                0x0 - <unknown>

---- editable_only stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: editable_only-2
Source: crates/uv/tests/pip_list.rs:271
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-Package         Version Editable project location                                
    4       │---------------- ------- ---------------------------------------------------------
          3 │+Package         Version Editable project location                                                                     
          4 │+--------------- ------- ----------------------------------------------------------------------------------------------
    5     5 │ poetry-editable 0.1.0   [WORKSPACE_DIR]/scripts/editable-installs/poetry_editable
    6     6 │ 
    7     7 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'editable_only' panicked at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:563:9:
snapshot assertion for 'editable_only-2' failed in line 271
stack backtrace:
   0:     0x5613b3cf18dc - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1ffcaa4041542985
   1:     0x5613b3d1f1d0 - core::fmt::write::h1b82113e9b693b22
   2:     0x5613b3cee12f - std::io::Write::write_fmt::hd588a8689be4a0fb
   3:     0x5613b3cf16c4 - std::sys_common::backtrace::print::h08a5d46ff202da96
   4:     0x5613b3cf3407 - std::panicking::default_hook::{{closure}}::h0dc7fd1af3fefd78
   5:     0x5613b3cf30f3 - std::panicking::default_hook::hdbcb9fe0d99a03f5
   6:     0x5613b3621217 - test::test_main::{{closure}}::hecd46e023328d5e6
   7:     0x5613b3cf3a18 - std::panicking::rust_panic_with_hook::heb55276fcd948217
   8:     0x5613b3cf376e - std::panicking::begin_panic_handler::{{closure}}::hf186713900a95986
   9:     0x5613b3cf1da6 - std::sys_common::backtrace::__rust_end_short_backtrace::h01811d7a116e9704
  10:     0x5613b3cf34d2 - rust_begin_unwind
  11:     0x5613b3483b25 - core::panicking::panic_fmt::h0827c44294bd0552
  12:     0x5613b34aa03a - insta::runtime::finalize_assertion::ha4af970e8c318200
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:563:9
  13:     0x5613b34aac1b - insta::runtime::assert_snapshot::hb2c3593f1a320f6f
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/cargo_home/gentoo/insta-1.35.1/src/runtime.rs:680:9
  14:     0x5613b3494732 - pip_list::editable_only::ha94a61278e6d14e7
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/crates/uv/tests/pip_list.rs:271:5
  15:     0x5613b3492aa7 - pip_list::editable_only::{{closure}}::he336388d3f033acb
                               at /var/tmp/portage/dev-python/uv-0.1.13/work/uv-0.1.13/crates/uv/tests/pip_list.rs:199:23
  16:     0x5613b3498f06 - core::ops::function::FnOnce::call_once::h160e78508ef75fd3
                               at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5
  17:     0x5613b36268df - test::__rust_begin_short_backtrace::h22539721e88f932b
  18:     0x5613b3625885 - test::run_test::{{closure}}::h07bc5892fe0b0b84
  19:     0x5613b35edb46 - std::sys_common::backtrace::__rust_begin_short_backtrace::h94357c6f10d6d2ec
  20:     0x5613b35f2bf7 - core::ops::function::FnOnce::call_once{{vtable.shim}}::h768f50f0af66b68b
  21:     0x5613b3cf9bc5 - std::sys::unix::thread::Thread::new::thread_start::h1abe540572f6a146
  22:     0x7f41c6e824d1 - start_thread
                               at /tmp/portage/sys-libs/glibc-2.39-r2/work/glibc-2.39/nptl/pthread_create.c:447:8
  23:     0x7f41c6effff8 - __GI___clone3
                               at /tmp/portage/sys-libs/glibc-2.39-r2/work/glibc-2.39/misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78
  24:                0x0 - <unknown>


failures:
    editable
    editable_only
    exclude

test result: FAILED^O. 2 passed; 3 failed; 0 ignored; 0 measured; 0 filtered out; finished in 7.38s

error: test failed, to rerun pass `--test pip_list`
```

---

_Comment by @mgorny on 2024-03-06 09:03_

Hmm, I'm going to guess it's because of all these escaped dashes and dots:

```
375         let workspace_len_difference = workspace_dir.as_str().len() + 32 - 16 - prefix.len();
(gdb) p workspace_dir.as_str()
$1 = "file:///var/tmp/portage/dev\\-python/uv\\-0\\.1\\.13/work/uv\\-0\\.1\\.13/"
```

---

_Comment by @mgorny on 2024-03-06 09:46_

I'm going to try making a pull request.

---

_Referenced in [astral-sh/uv#2237](../../astral-sh/uv/pulls/2237.md) on 2024-03-06 10:03_

---

_Closed by @charliermarsh on 2024-03-06 13:32_

---

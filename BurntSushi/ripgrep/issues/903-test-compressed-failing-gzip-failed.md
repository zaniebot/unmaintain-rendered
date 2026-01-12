```yaml
number: 903
title: test compressed_failing_gzip failed
type: issue
state: closed
author: misery
labels:
  - bug
assignees: []
created_at: 2018-05-01T14:09:23Z
updated_at: 2018-07-22T15:09:07Z
url: https://github.com/BurntSushi/ripgrep/issues/903
synced_at: 2026-01-12T16:13:22Z
```

# test compressed_failing_gzip failed

---

_@misery_

#### What version of ripgrep are you using?
0.8.1

#### How did you install ripgrep?
Trying a package the version. 0.7.1 compiles without problems and test was succesful.

https://github.com/alpinelinux/aports/pull/1250

#### What operating system are you using ripgrep on?
AlpineLinux

#### Describe your question, feature request, or bug.

See:
https://travis-ci.org/alpinelinux/aports/builds/373477970

```
---- compressed_failing_gzip stdout ----
        thread 'compressed_failing_gzip' panicked at 'assertion failed: `(left == right)`
  left: `false`,
 right: `true`', tests/tests.rs:1769:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at src/libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at src/libstd/sys_common/backtrace.rs:68
             at src/libstd/sys_common/backtrace.rs:57
   2: std::panicking::default_hook::{{closure}}
             at src/libstd/panicking.rs:381
   3: std::panicking::default_hook
             at src/libstd/panicking.rs:391
   4: std::panicking::rust_panic_with_hook
             at src/libstd/panicking.rs:577
   5: std::panicking::begin_panic
             at src/libstd/panicking.rs:538
   6: std::panicking::begin_panic_fmt
             at src/libstd/panicking.rs:522
   7: integration::compressed_failing_gzip
             at tests/tests.rs:1769
   8: <F as alloc::boxed::FnBox<A>>::call_box
             at src/libtest/lib.rs:1449
             at /home/buildozer/aports/community/rust/src/rustc-1.24.1-src/src/libcore/ops/function.rs:223
             at /home/buildozer/aports/community/rust/src/rustc-1.24.1-src/src/liballoc/boxed.rs:815
   9: __rust_maybe_catch_panic
             at src/libpanic_unwind/lib.rs:101


failures:
    compressed_failing_gzip

test result: FAILED. 155 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out

```

#### If this is a bug, what are the steps to reproduce the behavior?
Try to build ripgrep by abuild of AlpineLinux.



---

_Comment by @BurntSushi on 2018-05-01 14:23_

This particular test is checking that ripgrep handles files with a `.gz` extension that aren't valid gzip correctly. In particular, it makes sure that ripgrep surfaces the error message. This particular test checks for a specific string emitted by gzip, namely, `not in gzip format`. For example, from the root of this repo:

```
$ gzip -d -c tests/hay.rs
gzip: tests/hay.rs: not in gzip format
$ gzip --version
gzip 1.9
Copyright (C) 2017 Free Software Foundation, Inc.
Copyright (C) 1993 Jean-loup Gailly.
This is free software.  You may redistribute copies of it under the terms of
the GNU General Public License <https://www.gnu.org/licenses/gpl.html>.
There is NO WARRANTY, to the extent permitted by law.
Written by Jean-loup Gailly.
```

What is the output on your system? I don't know much about Alpine, but if it's using a different version of gzip, then my guess is that the test is too constrained in its expectations. We should probably modify it so that it checks that stderr is non-empty.

---

_Label `bug` added by @BurntSushi on 2018-05-01 14:23_

---

_Comment by @misery on 2018-05-01 14:29_

Ah, ok, I understand. Alpine uses busybox as default.

BusyBox v1.28.2 (2018-04-02 10:33:36 UTC) multi-call binary.
```
$ gzip -d -c tests/hay.rs
gzip: invalid magic
```


---

_Closed by @BurntSushi on 2018-07-22 15:09_

---

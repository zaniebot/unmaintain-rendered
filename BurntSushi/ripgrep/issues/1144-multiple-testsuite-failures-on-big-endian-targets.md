```yaml
number: 1144
title: Multiple testsuite failures on big-endian targets
type: issue
state: closed
author: glaubitz
labels:
  - question
assignees: []
created_at: 2018-12-16T16:12:36Z
updated_at: 2019-02-10T09:56:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1144
synced_at: 2026-01-12T16:13:23Z
```

# Multiple testsuite failures on big-endian targets

---

_@glaubitz_

The testsuite of ```ripgrep``` 0.10.0 is failing on all of Debian's big-endian targets:

```
failures:

---- feature::f34_only_matching_line_column stdout ----
thread 'feature::f34_only_matching_line_column' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sherlock:1:57:Sherlock
sherlock:3:49:Sherlock

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sherlock:1:57:Sherlock
sherlock:2:49:Sherlock

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/feature.rs:110:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::feature::f34_only_matching_line_column::{{closure}}
             at tests/feature.rs:110
   8: integration::feature::f34_only_matching_line_column
             at tests/macros.rs:7
   9: integration::feature::f34_only_matching_line_column::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- feature::f917_trim stdout ----
thread 'feature::f917_trim' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2-For the Doctor Watsons of this world, as opposed to the Sherlock
3:Holmeses, success in the province of detective work must always
4-be, to a very large extent, the result of luck. Sherlock Holmes
5-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2-For the Doctor Watsons of this world, as opposed to the Sherlock
2:Holmeses, success in the province of detective work must always
2-be, to a very large extent, the result of luck. Sherlock Holmes
2-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/feature.rs:591:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::feature::f917_trim::{{closure}}
             at tests/feature.rs:591
   8: integration::feature::f917_trim
             at tests/macros.rs:7
   9: integration::feature::f917_trim::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- feature::f917_trim_match stdout ----
thread 'feature::f917_trim_match' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2-For the Doctor Watsons of this world, as opposed to the Sherlock
3:Holmeses, success in the province of detective work must always
4-be, to a very large extent, the result of luck. Sherlock Holmes
5-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2-For the Doctor Watsons of this world, as opposed to the Sherlock
2:Holmeses, success in the province of detective work must always
2-be, to a very large extent, the result of luck. Sherlock Holmes
2-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/feature.rs:619:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::feature::f917_trim_match::{{closure}}
             at tests/feature.rs:619
   8: integration::feature::f917_trim_match
             at tests/macros.rs:7
   9: integration::feature::f917_trim_match::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- json::basic stdout ----
thread 'json::basic' panicked at 'assertion failed: `(left == right)`
  left: `Context { path: Some(Text { text: "sherlock" }), lines: Text { text: "Holmeses, success in the province of detective work must always\n" }, line_number: Some(1), absolute_offset: 65, submatches: [] }`,
 right: `Context { path: Some(Text { text: "sherlock" }), lines: Text { text: "Holmeses, success in the province of detective work must always\n" }, line_number: Some(2), absolute_offset: 65, submatches: [] }`', tests/json.rs:151:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::json::basic::{{closure}}
             at tests/json.rs:151
   8: integration::json::basic
             at tests/macros.rs:7
   9: integration::json::basic::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::after_context_line_numbers stdout ----
thread 'misc::after_context_line_numbers' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
2-Holmeses, success in the province of detective work must always
3:be, to a very large extent, the result of luck. Sherlock Holmes
4-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
1-Holmeses, success in the province of detective work must always
2:be, to a very large extent, the result of luck. Sherlock Holmes
3-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:427:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::after_context_line_numbers::{{closure}}
             at tests/misc.rs:427
   8: integration::misc::after_context_line_numbers
             at tests/macros.rs:7
   9: integration::misc::after_context_line_numbers::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::before_context_line_numbers stdout ----
thread 'misc::before_context_line_numbers' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
2-Holmeses, success in the province of detective work must always
3:be, to a very large extent, the result of luck. Sherlock Holmes

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
1-Holmeses, success in the province of detective work must always
2:be, to a very large extent, the result of luck. Sherlock Holmes

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:451:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::before_context_line_numbers::{{closure}}
             at tests/misc.rs:451
   8: integration::misc::before_context_line_numbers
             at tests/macros.rs:7
   9: integration::misc::before_context_line_numbers::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::columns stdout ----
thread 'misc::columns' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:57:For the Doctor Watsons of this world, as opposed to the Sherlock
3:49:be, to a very large extent, the result of luck. Sherlock Holmes

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:57:For the Doctor Watsons of this world, as opposed to the Sherlock
2:49:be, to a very large extent, the result of luck. Sherlock Holmes

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:49:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::columns::{{closure}}
             at tests/misc.rs:49
   8: integration::misc::columns
             at tests/macros.rs:7
   9: integration::misc::columns::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::context_line_numbers stdout ----
thread 'misc::context_line_numbers' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
2-Holmeses, success in the province of detective work must always
--
5-but Doctor Watson has to have it taken out for him and dusted,
6:and exhibited clearly, with a label attached.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
1-Holmeses, success in the province of detective work must always
--
3-but Doctor Watson has to have it taken out for him and dusted,
3:and exhibited clearly, with a label attached.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:479:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::context_line_numbers::{{closure}}
             at tests/misc.rs:479
   8: integration::misc::context_line_numbers
             at tests/macros.rs:7
   9: integration::misc::context_line_numbers::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::inverted_line_numbers stdout ----
thread 'misc::inverted_line_numbers' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
2:Holmeses, success in the province of detective work must always
4:can extract a clew from a wisp of straw or a flake of cigar ash;
5:but Doctor Watson has to have it taken out for him and dusted,
6:and exhibited clearly, with a label attached.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:Holmeses, success in the province of detective work must always
3:can extract a clew from a wisp of straw or a flake of cigar ash;
3:but Doctor Watson has to have it taken out for him and dusted,
3:and exhibited clearly, with a label attached.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:121:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::inverted_line_numbers::{{closure}}
             at tests/misc.rs:121
   8: integration::misc::inverted_line_numbers
             at tests/macros.rs:7
   9: integration::misc::inverted_line_numbers::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::line_numbers stdout ----
thread 'misc::line_numbers' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
3:be, to a very large extent, the result of luck. Sherlock Holmes

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:For the Doctor Watsons of this world, as opposed to the Sherlock
2:be, to a very large extent, the result of luck. Sherlock Holmes

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:38:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::line_numbers::{{closure}}
             at tests/misc.rs:38
   8: integration::misc::line_numbers
             at tests/macros.rs:7
   9: integration::misc::line_numbers::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- misc::vimgrep stdout ----
thread 'misc::vimgrep' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sherlock:1:16:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:1:57:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:3:49:be, to a very large extent, the result of luck. Sherlock Holmes
sherlock:5:12:but Doctor Watson has to have it taken out for him and dusted,

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sherlock:1:16:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:1:57:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:2:49:be, to a very large extent, the result of luck. Sherlock Holmes
sherlock:3:12:but Doctor Watson has to have it taken out for him and dusted,

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/misc.rs:775:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::misc::vimgrep::{{closure}}
             at tests/misc.rs:775
   8: integration::misc::vimgrep
             at tests/macros.rs:7
   9: integration::misc::vimgrep::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- multiline::context stdout ----
thread 'multiline::context' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1-For the Doctor Watsons of this world, as opposed to the Sherlock
2:Holmeses, success in the province of detective work must always
3:be, to a very large extent, the result of luck. Sherlock Holmes
4-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1-For the Doctor Watsons of this world, as opposed to the Sherlock
1:Holmeses, success in the province of detective work must always
2:be, to a very large extent, the result of luck. Sherlock Holmes
3-can extract a clew from a wisp of straw or a flake of cigar ash;

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/multiline.rs:108:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::multiline::context::{{closure}}
             at tests/multiline.rs:108
   8: integration::multiline::context
             at tests/macros.rs:7
   9: integration::multiline::context::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- multiline::only_matching stdout ----
thread 'multiline::only_matching' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:Watson
1:Sherlock
2:Holmes
3:Sherlock Holmes
5:Watson

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1:Watson
1:Sherlock
2:Holmes
2:Sherlock Holmes
3:Watson

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/multiline.rs:59:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::multiline::only_matching::{{closure}}
             at tests/multiline.rs:59
   8: integration::multiline::only_matching
             at tests/macros.rs:7
   9: integration::multiline::only_matching::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic

---- multiline::vimgrep stdout ----
thread 'multiline::vimgrep' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sherlock:1:16:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:1:57:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:2:57:Holmeses, success in the province of detective work must always
sherlock:3:49:be, to a very large extent, the result of luck. Sherlock Holmes
sherlock:5:12:but Doctor Watson has to have it taken out for him and dusted,

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sherlock:1:16:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:1:57:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:2:57:Holmeses, success in the province of detective work must always
sherlock:2:49:be, to a very large extent, the result of luck. Sherlock Holmes
sherlock:3:12:but Doctor Watson has to have it taken out for him and dusted,

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests/multiline.rs:77:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: std::panicking::begin_panic_fmt
   7: integration::multiline::vimgrep::{{closure}}
             at tests/multiline.rs:77
   8: integration::multiline::vimgrep
             at tests/macros.rs:7
   9: integration::multiline::vimgrep::{{closure}}
             at tests/macros.rs:5
  10: core::ops::function::FnOnce::call_once
             at libcore/ops/function.rs:238
  11: <F as alloc::boxed::FnBox<A>>::call_box
  12: __rust_maybe_catch_panic


failures:
    feature::f34_only_matching_line_column
    feature::f917_trim
    feature::f917_trim_match
    json::basic
    misc::after_context_line_numbers
    misc::before_context_line_numbers
    misc::columns
    misc::context_line_numbers
    misc::inverted_line_numbers
    misc::line_numbers
    misc::vimgrep
    multiline::context
    multiline::only_matching
    multiline::vimgrep

test result: FAILED. 173 passed; 14 failed; 0 ignored; 0 measured; 0 filtered out
```

Full build logs here: https://buildd.debian.org/status/package.php?p=rust-ripgrep&suite=sid

Click on the "Build-Attempted" fields in the "Status" column for more information.

For access to a big-endian Linux machine to be able to debug the issue, you can request an account from the gcc compile farm (https://gcc.gnu.org/wiki/CompileFarm) and test on Linux/sparc64, for example.

---

_Comment by @BurntSushi on 2018-12-16 16:33_

That's interesting. Nothing immediately comes to mind as to the root cause.

I don't know when I'll have time to look into this. (I do not have immediate access to a big endian machine I can test on, and I don't know when I'll be able to go through the process of requesting a machine.)

---

_Label `bug` added by @BurntSushi on 2018-12-16 16:35_

---

_Comment by @BurntSushi on 2018-12-16 16:36_

It looks like all of the failures are related to the line number emitted for each match. That's quite baffling as there shouldn't be any endian specific code in that path!

---

_Comment by @sylvestre on 2019-01-04 10:06_

@BurntSushi do you know who could help with that? 
This is blocking ripgrep 0.10 to move to Debian testing and the freeze is coming very soon. Thanks

---

_Comment by @BurntSushi on 2019-01-04 11:59_

I don't know. Anyone who has access to a big endian machine and can debug Rust could potentially help. Please note that I have virtually zero insight into when, where or why ripgrep is blocked by something in Debian, so it's very difficult for me to prioritize something like this when perhaps I should have.

One thing that might help is if someone wrote out the steps for using one of the big-endian machines to build the same version of ripgrep that this bug was found on using the compiler farm. I've just requested an account.

Another option might be to add a big endian target to the CI config and see what happens. e.g., https://github.com/BurntSushi/byteorder/blob/803136cc2719cc4a5baf562877e01938267a6bd9/.travis.yml#L8-L11

---

_Comment by @BurntSushi on 2019-01-04 12:11_

Oof. I have a hypothesis. I bet the issue is in my rewrite of the `memchr` crate, since that's involved in line counting, isn't tested on a big endian arch _and_ is susceptible to endian related bugs.

---

_Comment by @BurntSushi on 2019-01-04 13:23_

OK, I can't seem to reproduce this issue. After logging into `gcc202` and [applying this work-around to get Cargo to work](https://github.com/rust-lang/cargo/issues/6471), I was able to run ripgrep's test suite successfully. memchr also appears to be doing fine on big endian: https://github.com/BurntSushi/rust-memchr/pull/45

I'm not sure where to go from here.

---

_Comment by @sylvestre on 2019-01-04 14:37_

@glaubitz rings a bell? :)


---

_Comment by @glaubitz on 2019-01-04 14:46_

@sylvestre I'll test on zelenka (s390x). I would be surprised if the bug just went away.

---

_Comment by @glaubitz on 2019-01-04 14:54_

Ok, ```ripgrep``` from git works indeed fine. I can give-back the package on the buildds, maybe the bug was in one of the dependencies.

---

_Comment by @BurntSushi on 2019-01-04 14:58_

@glaubitz Interesting. If you can get a list of dependency versions in which the test failures occurred, I can try testing that config. One of the things that happened somewhat recently was a memchr upgrade (from an old version to a rewrite). I wonder if that's the culprit.

Unfortunately, [this bug](https://github.com/rust-lang/cargo/issues/6471) is making it difficult for me to make progress. To clarify, every time Cargo tries to update its index, I'm getting this error, e.g.,

```
$ cargo update -p memchr --precise 2.0.2
    Updating crates.io index
error: Unable to update registry `https://github.com/rust-lang/crates.io-index`

Caused by:
  failed to fetch `https://github.com/rust-lang/crates.io-index`

Caused by:
  the SSL certificate is invalid: 0x08 - The certificate is not correctly signed by the trusted CA; class=Ssl (16); code=Certificate (-17)
```

---

_Comment by @glaubitz on 2019-01-04 15:02_

@BurntSushi Let me apply a workaround. The bug you linked actually goes away when building ```cargo``` from upstream. It seems to be a bug in Debian's ```cargo``` package on sparc64.

---

_Comment by @BurntSushi on 2019-01-04 15:03_

@glaubitz Thanks!

---

_Comment by @glaubitz on 2019-01-04 15:13_

@BurntSushi Can you try again?

---

_Comment by @BurntSushi on 2019-01-04 15:24_

Hey it works! Thanks!

So I just tried pinning `memchr` to an older version, but tests passed. I then checked out a version of ripgrep from the beginningish of this month (`f81b727`) and tests passed there too.

Here's a question: do the builds that are failing build with PCRE2 support? I can't seem to compile `grep-pcre2` on this sparc machine, but I'm a bit out of my depth there. I ask because if PCRE2 is successfully being compiled and built, then most of the integration tests run twice: once with the default regex engine and another with PCRE2. I can't quite imagine how that could be causing the specific test failures shown above though. I'm mostly just trying to nail down any other differences I can think of.

---

_Comment by @BurntSushi on 2019-01-24 01:33_

@glaubitz Any updates on how I can reproduce this?

---

_Comment by @glaubitz on 2019-01-24 01:55_

You could try building the Debian source on a big-endian box.

Retrieve the source with:

```
$ dget -u http://deb.debian.org/debian/pool/main/r/rust-ripgrep/rust-ripgrep_0.10.0-1.dsc
```

---

_Comment by @BurntSushi on 2019-01-24 02:05_

Seems to work fine on gcc202. Test suite runs and passes.

The only other thing I can think of is whether y'all are building with PCRE2 support, because PCRE2 doesn't build on the sparc machine, so I can't test it.

---

_Comment by @glaubitz on 2019-01-24 02:10_

Regular ```pcre2``` builds fine on sparc64, unless there is another version you mean:

> https://buildd.debian.org/status/package.php?p=pcre2&suite=sid

---

_Comment by @BurntSushi on 2019-01-24 02:19_

If I run `cargo build --features pcre2`, then there's a bunch of compilation errors. ripgrep is probably trying to build its own vendored copy of PCRE2, and I wouldn't be surprised if I bungled that up. The next step there is to install pcre2 on the sparc machine such that `pkg-config --libs libpcre2-8` works, but I don't know how to do that.

Looking more closely at the sparc64 build log from your original comment, I see this is how tests are run:

```
debian cargo wrapper: running subprocess (['env', 'RUST_BACKTRACE=1', '/usr/bin/cargo', '-Zavoid-dev-deps', 'test', '--verbose', '--verbose', '-j32', '--target', 'sparc64-unknown-linux-gnu', '--all'],) {}
```

which means that PCRE2 isn't being compiled, so I don't think that's the issue.

So then I run

```
$ dget -u http://deb.debian.org/debian/pool/main/r/rust-ripgrep/rust-ripgrep_0.10.0-1.dsc
$ cd rust-ripgrep-0.10.0
$ cargo -Zavoid-dev-deps test --verbose --verbose -j32 --target sparc64-unknown-linux-gnu --all
```

And everything passes. Here's the test output: https://gist.github.com/ec8acb1394789a32bb44364e230c0966

Is there something I'm missing about the build environment? To be clear, here's the machine's info:

```
burntsushi@gcc202:~/rust-ripgrep-0.10.0$ uname -a
Linux gcc202 4.19.0-1-sparc64-smp #1 SMP Debian 4.19.9-1 (2018-12-16) sparc64 GNU/Linux
burntsushi@gcc202:~/rust-ripgrep-0.10.0$ lscpu
Architecture:        sparc64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Big Endian
CPU(s):              64
On-line CPU(s) list: 0-63
Thread(s) per core:  8
Core(s) per socket:  8
Socket(s):           1
Model name:          UltraSparc T5 (Niagara5)
Flags:               sun4v
```

---

_Label `question` added by @BurntSushi on 2019-01-31 12:19_

---

_Label `bug` removed by @BurntSushi on 2019-01-31 12:19_

---

_Comment by @BurntSushi on 2019-01-31 12:21_

This isn't reproducible as given (see previous comment), so I'm closing. If a reproduction can be found, then I'm happy to re-open.

---

_Closed by @BurntSushi on 2019-01-31 12:21_

---

_Comment by @plugwash on 2019-02-07 20:56_

I tried to reproduce this by logging into the Debian s390x porterbox, setting up a sid chroot, installing the build-dependencies and building the package. The test suite did fail but it failed in a totally different way to what was reported into the bug. Specifically it gave a bunch of "permission denied" errors.

https://bugs.debian.org/cgi-bin/bugreport.cgi?att=1;bug=916615;filename=rust-ripgrep.log;msg=19

Any thoughts on what might be going on here.

---

_Comment by @BurntSushi on 2019-02-07 22:16_

@plugwash ripgrep's integration test suite uses `/tmp` to setup test directories in which to run tests. From that output, it looks like it's not allowed to write files there? If writing to `/tmp` is an issue, you should be able to change which scratch directory is used via setting the `TMPDIR` environment variable.

---

_Comment by @BurntSushi on 2019-02-07 22:17_

N.B. I'd be happy to try and debug any test failure here, but given my unfamiliarity with Debian and its infrastructure, I'd kindly request that if you'd like me to do that, that you write out the set of steps I need to take (and the commands I need to run) a bit more verbosely.

---

_Comment by @plugwash on 2019-02-08 00:05_

I'm a Debian guy but not a rust guy, so I know how Debian stuff in general works but I don't know about rust stuff or the rust->debian packaging layers :/

I confirmed that the issue with nearly all the tests failing was caused by use of a fixed directory name in /tmp . This meant that my build of the package was trying to use the same temporary directory as someone else who had tried to build the package on the same porterbox.

After dealing with that I confirmed that the original issue was still present 

Basically I created a debian sid chroot for testing in, downloaded the source with apt-get source, installed the build-dependencies with apt-get build-dep and built the package with dpkg-buildpackage . The steps to recreate what I did if you have access to Debian porterboxes would be basically

ssh user@zelenka.debian.org
#create a chroot
schroot -b -c sid -n burntsushi-sid
#update it
dd-schroot-cmd -c burntsushi-sid apt-get update
dd-schroot-cmd -c burntsushi-sid apt-get upgrade
#install the build-dependencies
dd-schroot-cmd -c burntsushi-sid apt-get build-dep rust-ripgrep
#get patch to prevent tempdir conflicts
wget -O rust-ripgrep.debdiff 'https://bugs.debian.org/cgi-bin/bugreport.cgi?att=1;bug=921693;filename=rust-ripgrep.debdiff;msg=5'
#enter the chroot
schroot -r -c burntsushi-sid
#get the source for rust-ripgrep
apt-get source rust-ripgrep
cd rust-ripgrep-0.10.0
#apply patch to prevent tempdir conflicts
patch -p1 < ../rust-ripgrep.debdiff
#build the package
dpkg-buildpackage

I also tried doing an "upstream build" of rust-ripgrep by cloning the latest source and building/testing it according to the instructions in the readme. That also failed in a "new and exciting" way with

error: failed to run custom build command for `pcre2-sys v0.1.1`
process didn't exit successfully: `/home/plugwash/ripgrep/target/debug/build/pcre2-sys-d98e2658802bf9ff/build-script-build` (exit code: 101)
--- stdout
cargo:rerun-if-env-changed=PCRE2_SYS_STATIC

--- stderr
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory" }', src/libcore/result.rs:1009:5
note: Run with `RUST_BACKTRACE=1` for a backtrace.

warning: build failed, waiting for other jobs to finish...

---

_Comment by @BurntSushi on 2019-02-08 00:38_

Thanks so much for investigating!

I don't think I know what a Debian porterbox is unfortunately. I was able to get an account on the GCC "compile farm" for sparc, but I couldn't reproduce the issue there. On that sparc machine, I was able to clone the ripgrep repo and run all tests successfully.

Is there a way to get access to the same machine you have so that I can try to debug the test failures?

> I also tried doing an "upstream build" of rust-ripgrep by cloning the latest source and building/testing it according to the instructions in the readme. That also failed in a "new and exciting" way with

The PCRE2 error is interesting. Did you use `cargo test --all` or `cargo test --all --features pcre2`? I had thought Debian used the former, and if you do use the former, then `pcre2-sys` shouldn't be getting built at all since it's an optional dependency.

---

_Comment by @plugwash on 2019-02-08 01:10_

Debian porterboxes are system's provided for the porting of Debian there is a process for non-dds to request access, but I don't know how well it works in practice.

https://dsa.debian.org/doc/guest-account/

> Did you use cargo test --all or cargo test --all --features pcre2?

The former, but I just tried the latter too, didn't seem to make any difference.


---

_Comment by @BurntSushi on 2019-02-08 11:44_

@plugwash Thanks for the link! Unfortunately, I have no idea how to follow the instructions in that link to get a guest account. :-( It sounds like I need to go find someone to sponsor me, but that page has about several different initialisms that are mysterious to me.

It sounds like we're stuck until someone who can reproduce this can also debug it. I'm still happy to do it myself, but my next steps aren't clear.

> The former, but I just tried the latter too, didn't seem to make any difference.

The former shouldn't be building pcre2 at all, so something strange is happening.

---

_Comment by @sylvestre on 2019-02-09 14:46_

Updating to memchr 2.1.3 [doesn't fix the issue](https://buildd.debian.org/status/fetch.php?pkg=rust-ripgrep&arch=mips&ver=0.10.0-1&stamp=1549718149&raw=0)

---

_Comment by @BurntSushi on 2019-02-09 14:51_

Yeah, it was just a guess. It makes sense that it doesn't fix it in retrospect, since the only endian specific code in memchr is for Intel targets. Everything else is endian agnostic.

---

_Comment by @sylvestre on 2019-02-09 14:55_

No worries, it was worth trying

---

_Comment by @infinity0 on 2019-02-09 18:21_

We don't have permission to write to `/tmp` on debian build machines and setting `TMPDIR` doesn't help either:

~~~~
(sid_s390x-dchroot)infinity0@zelenka:~/rust-ripgrep-0.10.0$ RUST_BACKTRACE=1 TMPDIR=$PWD/debian/tests ./target/s390x-unknown-linux-gnu/debug/integration-29a682bdb9920da3 regression::r206

running 1 test
test regression::r206 ... FAILED

failures:

---- regression::r206 stdout ----
thread 'regression::r206' panicked at 'called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory" }', src/libcore/result.rs:1009:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
   1: std::sys_common::backtrace::print
   2: std::panicking::default_hook::{{closure}}
   3: std::panicking::default_hook
   4: std::panicking::rust_panic_with_hook
   5: std::panicking::continue_panic_fmt
   6: rust_begin_unwind
   7: core::panicking::panic_fmt
   8: core::result::unwrap_failed
             at /usr/src/rustc-1.32.0/src/libcore/macros.rs:26
   9: <core::result::Result<T, E>>::unwrap
             at /usr/src/rustc-1.32.0/src/libcore/result.rs:808
  10: integration::util::TestCommand::output
             at tests/util.rs:333
  11: integration::util::TestCommand::stdout
             at tests/util.rs:285
  12: integration::regression::r206::{{closure}}
             at tests/regression.rs:264
  13: integration::regression::r206
             at tests/macros.rs:7
  14: integration::regression::r206::{{closure}}
             at tests/macros.rs:5
  15: core::ops::function::FnOnce::call_once
             at /usr/src/rustc-1.32.0/src/libcore/ops/function.rs:238
  16: <F as alloc::boxed::FnBox<A>>::call_box
  17: __rust_maybe_catch_panic


failures:
    regression::r206

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 186 filtered out

~~~~

---

_Comment by @infinity0 on 2019-02-09 18:22_

Note that the file does actually exist and is created by the test:

~~~~
(sid_s390x-dchroot)infinity0@zelenka:~/rust-ripgrep-0.10.0$ ls -la debian/tests/ripgrep-tests/r206/0/foo/bar.txt
-rw-r--r-- 1 infinity0 infinity0 4 Feb  9 18:21 debian/tests/ripgrep-tests/r206/0/foo/bar.txt
~~~~

---

_Comment by @BurntSushi on 2019-02-09 18:24_

The /tmp issue isn't endian specific though. Why does it matter on s390x but not other targets?

---

_Comment by @infinity0 on 2019-02-09 18:27_

The `/tmp` issue is a different issue that is preventing me from debugging this issue. It probably happens everywhere now.

---

_Comment by @infinity0 on 2019-02-09 18:29_

Correction, it doesn't affect the build servers but it does affect the developer porterbox machines, that's why I'm hitting it first. Can you try to reproduce? On Debian we also set `--target` so the build scripts generalise to cross-compiling, so the build files are actually `target/$triplet/` - not sure if that's a problem.

---

_Comment by @infinity0 on 2019-02-09 18:33_

Looks like it's trying to run `"/home/infinity0/rust-ripgrep-0.10.0/target/s390x-unknown-linux-gnu/debug/../rg" "--path-separator" "/" "test" "-g" "*.txt"` which doesn't exist hence the error.

"/home/infinity0/rust-ripgrep-0.10.0/target/s390x-unknown-linux-gnu/debug/rg" is actually what exists.


---

_Comment by @infinity0 on 2019-02-09 18:50_

Looks like changing `../rg` to `rg` in `tests/utils.rs` plus setting `TMPDIR` allows the running the test to work directly. Why is it `../rg` in the test code?

---

_Comment by @infinity0 on 2019-02-09 18:52_

Actually I spoke too soon; running the test executable directly seems to work, but running it via cargo doesn't. Maybe it's screwing with `TMPDIR` or something.

---

_Comment by @infinity0 on 2019-02-09 18:57_

OK it looks like `TMPDIR` is fine, the actual problem is that running the tests through cargo requires the executable to be `../rg` but running the test directly requires it to be `rg` without the `../`. I've managed to reproduce the actual topic of this bug report, will follow up with more details later.

---

_Comment by @infinity0 on 2019-02-09 19:23_

I can also reproduce it using this git repository's `HEAD` using no Debian packages, on `zelenka.debian.org`, using rust's own s390x binaries. Please consider re-opening. This machine is:

~~~~
(sid_s390x-dchroot)infinity0@zelenka:~/ripgrep$ uname -a
Linux zelenka 4.9.0-8-s390x #1 SMP Debian 4.9.130-2 (2018-10-27) s390x GNU/Linux
(sid_s390x-dchroot)infinity0@zelenka:~/ripgrep$ cat /proc/cpuinfo
vendor_id       : IBM/S390
# processors    : 2
bogomips per cpu: 9057.00
max thread id   : 0
features        : esan3 zarch stfle msa ldisp eimm dfp edat etf3eh highgprs te vx sie
cache0          : level=1 type=Data scope=Private size=128K line_size=256 associativity=8
cache1          : level=1 type=Instruction scope=Private size=96K line_size=256 associativity=6
cache2          : level=2 type=Data scope=Private size=2048K line_size=256 associativity=8
cache3          : level=2 type=Instruction scope=Private size=2048K line_size=256 associativity=8
cache4          : level=3 type=Unified scope=Shared size=65536K line_size=256 associativity=16
cache5          : level=4 type=Unified scope=Shared size=491520K line_size=256 associativity=30
processor 0: version = FF,  identification = 01F467,  machine = 2964
processor 1: version = FF,  identification = 01F467,  machine = 2964

cpu number      : 0
cpu MHz dynamic : 5000
cpu MHz static  : 5000

cpu number      : 1
cpu MHz dynamic : 5000
cpu MHz static  : 5000
~~~~

---

_Comment by @BurntSushi on 2019-02-09 19:43_

I generally keep issues that aren't actionable closed. I'm happy to reopen, but to be clear, I'm blocked on debugging this myself because I don't have access to a machine that reproduces the problem.

---

_Reopened by @BurntSushi on 2019-02-09 19:43_

---

_Comment by @infinity0 on 2019-02-09 20:44_

The problem seems to stem from `grep-searcher`, we don't run tests for most rust crates for Debian so didn't notice this before. The test failures are similar, involving line-number differences:

~~~~
---- searcher::glue::tests::context_code1 stdout ----                                                   
thread 'searcher::glue::tests::context_code1' panicked at '                 
printed outputs differ! (label: reader-byline-noterm-number)                                                                                                         
                                                                               
expected:                                                           
\~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
4-33-                                                                                                                                                            
5-34-fn main() {                                                               
6:46:    let stdin = io::stdin();                                              
7-75-    let stdout = io::stdout();                                                 
8-106-                                                                    
9:107:    // Wrap the stdin reader in a Snappy reader.                     
10:156:    let mut rdr = snap::Reader::new(stdin.lock());                      
11-207-    let mut wtr = stdout.lock();                                        
12-240-    io::copy(&mut rdr, &mut wtr).expect("I/O operation failed");        
                                                                                                                                                                     
byte count:307                                                        
                                                                                    
\~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                                                               
got:                                                                           
\~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                         
3-33-                                                                                                                                                                 
4-34-fn main() {                                                                                        
4:46:    let stdin = io::stdin();                                              
4-75-    let stdout = io::stdout();                                                                                                                                
4-106-                                                                         
5:107:    // Wrap the stdin reader in a Snappy reader.                         
5:156:    let mut rdr = snap::Reader::new(stdin.lock());              
5-207-    let mut wtr = stdout.lock();                                         
5-240-    io::copy(&mut rdr, &mut wtr).expect("I/O operation failed");
                                                                                
byte count:307                                                                 
                                                                                                                                             
\~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', grep-searcher/src/testutil.rs:282:17                   
note: Run with `RUST_BACKTRACE=1` for a backtrace.    
~~~~

etc etc

---

_Comment by @infinity0 on 2019-02-09 20:55_

git bisect says 968491f8e93e506b224ee4c85bee85f7ead0e5d5 is the first bad commit

~~~~
git bisect start HEAD afa06c518a2bb255fde2a27ef0d401e28fbb0296
git bisect run sh -c 'cd grep-searcher && TMPDIR=$HOME/ripgrep/tests RUSTFLAGS="-L $LD_LIBRARY_PATH" RUSTDOCFLAGS="-L $LD_LIBRARY_PATH" cargo test'
[..]
968491f8e93e506b224ee4c85bee85f7ead0e5d5 is the first bad commit
commit 968491f8e93e506b224ee4c85bee85f7ead0e5d5
Author: Andrew Gallant <jamslam@gmail.com>
Date:   Sat Jan 19 09:52:10 2019 -0500

    deps: update to bytecount 0.5

    bytecount now uses runtime dispatch for enabling SIMD, which means we can
    no longer need the avx-accel features. We remove it from ripgrep since the
    next release will be a minor version bump, but leave them as no-ops for
    the crates that previously used it.

:100644 100644 ee8732ab0999fa383452dd34bced99fea317a932 854448b83af5aa047ccf5499ff62e4273a547790 M      Cargo.lock
:100644 100644 803aaf5b212217be38518283f7eb610ba7a82629 9d7e42d79f8c84ec90760c024c2c220b7910c49d M      Cargo.toml
:040000 040000 dff44c6c66e82c900b5621b0a3eca136fbf1ef9a 71afe73c63b09dc5d2892ee4f24e45355859f5fb M      grep-searcher
:040000 040000 22e40c7e38da7402d23673d86459c5307927d08b be6ffb1b136bba0cd8e34438571655243fb9b007 M      grep
bisect run success
~~~~

This updates from bytecount 0.3.2 to 0.5. In Debian we are using bytecount 0.4 and that is also bad. So it looks like the bug is in bytecount 0.4, 0.5 - will keep investigating.

---

_Comment by @BurntSushi on 2019-02-09 20:56_

Yeah, that makes sense.

Staring at that a bit more, I have a new hunch: the [crate I'm using to count lines](https://github.com/llogiq/bytecount) might be the culprit here. I'm not familiar with that code, but it's similarish to the kind of code that's in memchr, and that could in theory be endian dependent for $reasons. Hmmm... Running `bytecount`'s test suite on the sparc machine fails. cc @Veedrac @llogiq 

With that said, I still can't explain why the test suite passes on the gcc202 sparc machine, which is big-endian.

---

_Comment by @BurntSushi on 2019-02-09 20:57_

@infinity0 Ah you beat me to it! OK, that looks fairly compelling. I'll figure out how to send a bug report to `bytecount`. In the mean time, I can patch ripgrep (via the `grep-searcher` crate) to stop using `bytecount` on big endian machines.

---

_Comment by @infinity0 on 2019-02-09 20:58_

In fact reverting that commit on master (and deleting Cargo.lock to resolve the merge conflict) makes these errors go away.

---

_Comment by @BurntSushi on 2019-02-09 21:01_

OK, and now I have an explanation for why my sparc tests were passing. The `Cargo.lock` file seems to be using `bytecount 0.3.2`.

I never updated ripgrep to use `bytecount 0.4`, but did update it to `0.5` on Jan 19 of this year. That's well after this bug was reported. Is it possible the Debian was using `bytecount 0.4` even though ripgrep hadn't moved to it yet? It appears so! Looking at the original log reported in this ticket, it's using [`bytecount 0.4`](https://buildd.debian.org/status/fetch.php?pkg=rust-ripgrep&arch=sparc64&ver=0.10.0-1&stamp=1549716920&raw=0).

So it looks like one lesson to be learned here is to find a way to communicate more clearly that Debian might be using different versions of dependencies than what ripgrep's `Cargo.lock` is set to. I didn't realize that could happen.

---

_Comment by @infinity0 on 2019-02-09 21:02_

Thanks! I can take care of the bug report to bytecount, I can bisect that here too.

---

_Comment by @infinity0 on 2019-02-09 21:10_

> Debian might be using different versions of dependencies than what ripgrep's Cargo.lock is set to. I didn't realize that could happen.

Yes we should be a bit better about communicating that in future. Running tests would have helped to catch this earlier too, however it's a bit hard to do since "the easy way" (in Debian) would [introduce cyclic build-dependencies](https://salsa.debian.org/rust-team/debcargo/issues/15).


---

_Closed by @BurntSushi on 2019-02-09 21:13_

---

_Comment by @BurntSushi on 2019-02-09 21:14_

@infinity0 OK, master is updated with the patch, and I've published `grep-search 0.1.2` on crates.io with the fix.

---

_Comment by @BurntSushi on 2019-02-09 21:17_

@infinity0 I've also filed #1193 to see about catching these a bit more aggressively. It wouldn't have caught it initially, but my commit to bump bytecount would have failed at least. Thanks so much for debugging this!

---

_Comment by @llogiq on 2019-02-10 09:56_

Version 0.5.1 will have the fix. I will ping you once it's published.

---

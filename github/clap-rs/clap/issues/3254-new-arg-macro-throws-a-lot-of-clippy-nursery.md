---
number: 3254
title: "new `arg!` macro throws a lot of clippy nursery debug_assert_with_mut_call lints"
type: issue
state: closed
author: TDHolmes
labels:
  - C-bug
  - A-builder
  - S-triage
assignees: []
created_at: 2022-01-04T18:17:13Z
updated_at: 2022-01-11T17:10:36Z
url: https://github.com/clap-rs/clap/issues/3254
synced_at: 2026-01-07T13:12:19-06:00
---

# new `arg!` macro throws a lot of clippy nursery debug_assert_with_mut_call lints

---

_Issue opened by @TDHolmes on 2022-01-04 18:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.0.1

### Minimal reproducible code

Using the `cargo` example:
```rust
fn main() {
    let app = clap::App::new("cargo")
        .bin_name("cargo")
        .setting(clap::AppSettings::SubcommandRequired)
        .subcommand(
            clap::app_from_crate!().name("example").arg(
                clap::arg!(--"manifest-path" <PATH>)
                    .required(false)
                    .allow_invalid_utf8(true),
            ),
        );
    let matches = app.get_matches();
    let matches = match matches.subcommand() {
        Some(("example", matches)) => matches,
        _ => unreachable!("clap should ensure we don't get here"),
    };
    let manifest_path = matches
        .value_of_os("manifest-path")
        .map(std::path::PathBuf::from);
    println!("{:?}", manifest_path);
}
```


### Steps to reproduce the bug with the above code

run `cargo clippy -- -W clippy::nursery` or `cargo clippy -- -W clippy::debug-assert-with-mut-call`

```
$ clap-repro git:(main) ✗ cargo clippy -- -W clippy::nursery
    Checking clap-repro v0.1.0 (/Users/tdh/projects/tmp/clap-repro)

warning: do not call a function with mutable arguments inside of `debug_assert!`
 --> src/main.rs:7:17
  |
7 |                 clap::arg!(--"manifest-path" <PATH>)
  |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = note: `-W clippy::debug-assert-with-mut-call` implied by `-W clippy::nursery`
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#debug_assert_with_mut_call
  = note: this warning originates in the macro `$crate::arg_impl` (in Nightly builds, run with -Z macro-backtrace for more info)
```

Adding macro backtrace we get:
```
warning: do not call a function with mutable arguments inside of `debug_assert!`
   --> /Users/tdh/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.3/src/macros.rs:401:14
    |
338 |  / macro_rules! arg_impl {
339 |  |     ( @string $val:ident ) => {
340 |  |         stringify!($val)
341 |  |     };
...    |
401 |  |             ({
    |  |______________^
402 | ||                 debug_assert_eq!($arg.get_value_names(), None, "Flags should precede values");
403 | ||                 debug_assert!(!$arg.is_set($crate::ArgSettings::MultipleOccurrences), "Flags should precede `...`");
404 | ||
...   ||
410 | ||                 arg.long(long)
411 | ||             })
    | ||_____________^
...    |
533 |  |     };
534 |  | }
    |  |_- in this expansion of `$crate::arg_impl!` (#2)
...
615 | /  macro_rules! arg {
616 | |      ( $name:ident: $($tail:tt)+ ) => {
617 | |          $crate::arg_impl! {
618 | |              @arg ($crate::Arg::new($crate::arg_impl! { @string $name })) $($tail)+
...   |
622 |            let arg = $crate::arg_impl! {
    |  ____________________-
623 |                @arg ($crate::Arg::default()) $($tail)+
624 | |          };
    | |__________- in this macro invocation (#2)
...
627 | |      }};
628 | |  }
    | |__- in this expansion of `clap::arg!` (#1)
    |
   ::: src/main.rs:7:17
    |
7   |                    clap::arg!(--"manifest-path" <PATH>)
    |                    ------------------------------------ in this macro invocation (#1)
    |
    = note: `-W clippy::debug-assert-with-mut-call` implied by `-W clippy::nursery`
    = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#debug_assert_with_mut_call
```

### Actual Behaviour

I get a clippy warning for each instance of `arg!` in my repo.

### Expected Behaviour

No (or as few as possible) clippy lints trigger.

### Additional Context

I couldn't find which function takes a mutable argument... not sure why this lint is triggering.

### Debug Output

N/A - clippy issue

---

_Label `C-bug` added by @TDHolmes on 2022-01-04 18:17_

---

_Comment by @TDHolmes on 2022-01-04 18:19_

[clippy link for this lint](https://rust-lang.github.io/rust-clippy/master/index.html#debug_assert_with_mut_call)

---

_Comment by @TDHolmes on 2022-01-04 18:21_

ah.. sounds like there are [known cases of false positives](https://github.com/rust-lang/rust-clippy/issues/5112). Perhaps this is another instance of a false positive? 

---

_Comment by @epage on 2022-01-04 18:25_

If that backtrace is accurate, I think so

The two calls in the span do not take `&mut`
- https://docs.rs/clap/latest/clap/struct.Arg.html#method.get_value_names
- https://docs.rs/clap/latest/clap/struct.Arg.html#method.is_set

---

_Label `A-builder` added by @epage on 2022-01-11 17:10_

---

_Label `S-triage` added by @epage on 2022-01-11 17:10_

---

_Comment by @epage on 2022-01-11 17:10_

Assuming this is a false positive and closing this out.  If there is new information otherwise, let us know and we can re-open!

---

_Closed by @epage on 2022-01-11 17:10_

---

```yaml
number: 3669
title: Clap 3.1.13 fails to build by unwrapping a None value
type: issue
state: closed
author: sondr3
labels:
  - C-bug
assignees: []
created_at: 2022-04-30T14:38:58Z
updated_at: 2022-05-01T01:21:52Z
url: https://github.com/clap-rs/clap/issues/3669
synced_at: 2026-01-12T16:14:15Z
```

# Clap 3.1.13 fails to build by unwrapping a None value

---

_@sondr3_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

3.1.13

### Minimal reproducible code

```rust
fn main() {
    todo!()
}
```


### Steps to reproduce the bug with the above code

Sorry for not being able to reproduce this, this only happens in this one project I have where I upgraded `clap` and not in a few others I have, see this PR: https://github.com/sondr3/git-ignore/pull/9. It seems to come from the `build.rs` file when generating shell completions when running with `RUST_BACKTRACES=1`:

```
❯ RUST_BACKTRACE=1 cargo c
   Compiling git-ignore-generator v1.2.0 (/home/sondre/Code/rust/git-ignore)
error: failed to run custom build command for `git-ignore-generator v1.2.0 (/home/sondre/Code/rust/git-ignore)`

Caused by:
  process didn't exit successfully: `/home/sondre/Code/rust/git-ignore/target/debug/build/git-ignore-generator-322665dc073b363c/build-script-build` (exit status: 101)
  --- stdout
  cargo:rerun-if-changed=src/cli.rs
  cargo:rerun-if-changed=man

  --- stderr
  thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/output/usage.rs:436:35
  stack backtrace:
     0: rust_begin_unwind
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/std/src/panicking.rs:584:5
     1: core::panicking::panic_fmt
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/panicking.rs:143:14
     2: core::panicking::panic
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/panicking.rs:48:5
     3: core::option::Option<T>::unwrap
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/option.rs:752:21
     4: clap::output::usage::Usage::get_required_usage_from::{{closure}}
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/output/usage.rs:436:25
     5: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &mut F>::call_once
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/ops/function.rs:280:13
     6: core::option::Option<T>::map
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/option.rs:906:29
     7: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::next
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/iter/adapters/map.rs:103:9
     8: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/alloc/src/vec/spec_from_iter_nested.rs:26:32
     9: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/alloc/src/vec/spec_from_iter.rs:33:9
    10: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/alloc/src/vec/mod.rs:2552:9
    11: core::iter::traits::iterator::Iterator::collect
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/iter/traits/iterator.rs:1778:9
    12: clap::output::usage::Usage::get_required_usage_from
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/output/usage.rs:428:24
    13: clap::build::command::App::_build_bin_names_internal
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/build/command.rs:4133:28
    14: clap::build::command::App::_build_bin_names_internal
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/build/command.rs:4197:17
    15: clap::build::command::App::_build_bin_names_internal
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/build/command.rs:4197:17
    16: clap::build::command::App::build
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.13/src/build/command.rs:4015:9
    17: clap_complete::generator::_generate
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_complete-3.1.2/src/generator/mod.rs:239:5
    18: clap_complete::generator::generate_to
               at /home/sondre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_complete-3.1.2/src/generator/mod.rs:186:5
    19: build_script_build::build_shell_completion
               at ./build.rs:18:9
    20: build_script_build::main
               at ./build.rs:49:5
    21: core::ops::function::FnOnce::call_once
               at /rustc/7737e0b5c4103216d6fd8cf941b7ab9bdbaace7c/library/core/src/ops/function.rs:227:5
  note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

### Actual Behaviour

I have verified that this only happens on `3.1.13`, using `3.1.10`, `3.1.11` and`3.1.12` does not crash. I did a quick look at the commits but can't pinpoint any culprits from a quick glance.

### Expected Behaviour

This worked in the previous patch release.

### Additional Context

If need be, I can try to bisect `clap` itself if you need more information. I haven't been able to create a minimal, crashing example.

### Debug Output

_No response_

---

_Label `C-bug` added by @sondr3 on 2022-04-30 14:38_

---

_Comment by @epage on 2022-05-01 00:02_

A minimal reproduction:
```rust
use clap::{arg, Command};

fn main() {
    let mut cmd = Command::new("ctest").subcommand(
        Command::new("subcmd").subcommand(
            Command::new("multi")
                .about("tests subcommands")
                .author("Kevin K. <kbknapp@gmail.com>")
                .version("0.1")
                .arg(arg!(
                    <FLAG>                    "tests flags"
                )),
        ),
    );
    cmd.build();
}
```

If I remove one level of subcommand, it does not reproeduce

---

_Comment by @epage on 2022-05-01 00:09_

Looks like #3667 exposed that we aren't recursing where we need to

---

_Referenced in [clap-rs/clap#3670](../../clap-rs/clap/pulls/3670.md) on 2022-05-01 00:17_

---

_Closed by @epage on 2022-05-01 01:20_

---

_Comment by @epage on 2022-05-01 01:21_

Released in v3.1.14

---

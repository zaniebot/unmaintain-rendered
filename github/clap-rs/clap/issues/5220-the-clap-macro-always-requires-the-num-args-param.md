---
number: 5220
title: "The clap macro always requires the `num_args` param for clap_complete to work"
type: issue
state: closed
author: hugoclrd
labels:
  - C-bug
assignees: []
created_at: 2023-11-22T18:19:49Z
updated_at: 2023-11-24T01:53:42Z
url: https://github.com/clap-rs/clap/issues/5220
synced_at: 2026-01-07T13:12:20-06:00
---

# The clap macro always requires the `num_args` param for clap_complete to work

---

_Issue opened by @hugoclrd on 2023-11-22 18:19_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.74.0

### Clap Version

4.4.8

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser, Subcommand};
use clap_complete::{Generator, Shell};

#[derive(Subcommand, PartialEq, Clone, Debug)]
enum Command {
    /// Create and scaffold a new project
    #[clap(name = "new", bin_name = "new")]
    New(GenerateProject),
}

#[derive(Parser, PartialEq, Clone, Debug)]
struct GenerateProject {
    /// Project's name
    // #[clap(num_args = 1)] // <---- num_args must be defined, even for bool
    pub name: String,
}

#[derive(Parser, PartialEq, Clone, Debug)]
#[clap(version = env!("CARGO_PKG_VERSION"), name = "clarinet", bin_name = "clarinet")]
struct Opts {
    #[clap(subcommand)]
    command: Command,
}

fn main() {
    let cmd = Opts::command();
    let mut output_buffer = Vec::new();
    Shell::Zsh.generate(&cmd, &mut output_buffer);
    assert!(output_buffer.len() > 0);
}
```


### Steps to reproduce the bug with the above code

`cargo run`

Here is my Cargo.toml
```
[package]
name = "hello_world"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = { version = "4.4.8", features = ["derive"] }
clap_complete = { version = "4.4.4" }
```

### Actual Behaviour

Throws this error:
```
thread 'main' panicked at.../clap_complete-4.4.4/src/generator/utils.rs:117:39:
built
```

Also, I had the error for `Shell::Zsh` but not for `Shell::Bash`

Because of the `expect("built")` at this line: https://github.com/clap-rs/clap/blob/master/clap_complete/src/generator/utils.rs#L117

### Expected Behaviour

Two ways to avoid it:
- The `num_args` should not be required
- The `clap` macro shouldn't compile if it panics

### Additional Context

I found this issue while migrating from clap@3 to clap@4, so I only have this error with clap_complete@4 (but it was working under clap_generate@3)

### Debug Output

```console
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/hello_world`
[  clap_complete::shells::zsh]  get_args_of
[  clap_complete::shells::zsh]  write_opts_of
[  clap_complete::shells::zsh]  write_flags_of;
[clap_complete::generator::utils]       flags: name=proj
[  clap_complete::shells::zsh]  write_positionals_of;
[  clap_complete::shells::zsh]  get_subcommands_of: Has subcommands...true
[clap_complete::generator::utils]       subcommands: name=proj
[clap_complete::generator::utils]       subcommands: Has subcommands...true
[clap_complete::generator::utils]       subcommands:iter: name=new, bin_name=new
[  clap_complete::shells::zsh]  get_subcommands_of:iter: parent=proj, name=new, bin_name=new
[  clap_complete::shells::zsh]  parser_of: p=proj, bin_name=new
[  clap_complete::shells::zsh]  parser_of: p=new, bin_name=new
[  clap_complete::shells::zsh]  get_args_of
[  clap_complete::shells::zsh]  write_opts_of
[  clap_complete::shells::zsh]  write_flags_of;
[clap_complete::generator::utils]       flags: name=new
thread 'main' panicked at <path_to_crates.io>/clap_complete-4.4.4/src/generator/utils.rs:117:39:
built
stack backtrace:
   0: rust_begin_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:597:5
   1: core::panicking::panic_fmt
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:72:14
   2: core::panicking::panic_display
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:168:5
   3: core::panicking::panic_str
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:152:5
   4: core::option::expect_failed
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/option.rs:1988:5
   5: core::option::Option<T>::expect
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/option.rs:898:21
   6: clap_complete::generator::utils::flags::{{closure}}
             at <path_to_crates.io>/clap_complete-4.4.4/src/generator/utils.rs:117:22
   7: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &mut F>::call_mut
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/ops/function.rs:294:13
   8: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::find
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/slice/iter/macros.rs:302:24
   9: <core::iter::adapters::filter::Filter<I,P> as core::iter::traits::iterator::Iterator>::next
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/iter/adapters/filter.rs:59:9
  10: <core::iter::adapters::cloned::Cloned<I> as core::iter::traits::iterator::Iterator>::next
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/iter/adapters/cloned.rs:40:9
  11: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/alloc/src/vec/spec_from_iter_nested.rs:26:32
  12: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/alloc/src/vec/spec_from_iter.rs:33:9
  13: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/alloc/src/vec/mod.rs:2749:9
  14: core::iter::traits::iterator::Iterator::collect
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/iter/traits/iterator.rs:2053:9
  15: clap_complete::generator::utils::flags
             at <path_to_crates.io>/clap_complete-4.4.4/src/generator/utils.rs:116:5
  16: clap_complete::shells::zsh::write_flags_of
             at <path_to_crates.io>/clap_complete-4.4.4/src/shells/zsh.rs:540:14
  17: clap_complete::shells::zsh::get_args_of
             at <path_to_crates.io>/clap_complete-4.4.4/src/shells/zsh.rs:324:17
  18: clap_complete::shells::zsh::get_subcommands_of
             at <path_to_crates.io>/clap_complete-4.4.4/src/shells/zsh.rs:236:31
  19: <clap_complete::shells::zsh::Zsh as clap_complete::generator::Generator>::generate
             at <path_to_crates.io>/clap_complete-4.4.4/src/shells/zsh.rs:54:31
  20: <clap_complete::shells::shell::Shell as clap_complete::generator::Generator>::generate
             at <path_to_crates.io>/clap_complete-4.4.4/src/shells/shell.rs:89:27
  21: hello_world::main
             at ./src/main.rs:28:5
  22: core::ops::function::FnOnce::call_once
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Label `C-bug` added by @hugoclrd on 2023-11-22 18:19_

---

_Referenced in [stx-labs/clarinet#1263](../../stx-labs/clarinet/pulls/1263.md) on 2023-11-22 19:01_

---

_Comment by @epage on 2023-11-22 19:30_

If you follow the [example](https://docs.rs/clap_complete/latest/clap_complete/#example), you need to be using the top-level `generate` and `generate_to` functions for the relevant invariants to be prepared.

---

_Comment by @hugoclrd on 2023-11-23 16:05_

Thanks for your answer @epage.
Turns out that using `generate` fixed my issues with zsh but lead to more issues in bash, but I was able to debug that more easily.

When looking  at the example in the doc of clap_complete I was a bit confused because the first arg it takes is "generator", but you actually need to pass a `Shell`, but I guess it's kind of the same thing in this context.

I have a question that isn't related to the issue but probably because of the way the `clap` macro is used in this project, with the code above, the `help` and `version` command return an error
```rust
    let r = cmd.try_get_matches_from_mut(["proj", "new", "ok"]);
    println!("r: {}", r.is_ok()); // true
    let r = cmd.try_get_matches_from_mut(["proj", "help"]);
    println!("r: {}", r.is_ok()); // false
    let r = cmd.try_get_matches_from_mut(["proj", "version"]);
    println!("r: {}", r.is_ok()) // false
```

In the terminal, I do get the right help or version, but displayed in red. Any idea why? How I can fix?
Thx


---

_Comment by @hugoclrd on 2023-11-23 16:05_

Let me know if I should rather ask on SO or in the discussions maybe?

I can close this issue anyway, thanks for your help

---

_Closed by @hugoclrd on 2023-11-23 16:05_

---

_Comment by @epage on 2023-11-24 01:53_

> In the terminal, I do get the right help or version, but displayed in red. Any idea why? How I can fix?

That is very strange.  Feel free to start an issue or discussion with a full code sample.

btw
```rust
#[clap(version = env!("CARGO_PKG_VERSION"), name = "clarinet", bin_name = "clarinet")]
```
can be written as
```rust
#[clap(version, name = "clarinet", bin_name = "clarinet")]
```

---

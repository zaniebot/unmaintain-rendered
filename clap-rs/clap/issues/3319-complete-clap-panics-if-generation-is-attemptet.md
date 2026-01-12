```yaml
number: 3319
title: "[complete] clap panics if generation is attemptet without having `bin_name` set for app and subcommands (non-descriptive errors)"
type: issue
state: closed
author: hasezoey
labels:
  - C-bug
assignees: []
created_at: 2022-01-19T10:41:57Z
updated_at: 2022-01-20T07:28:26Z
url: https://github.com/clap-rs/clap/issues/3319
synced_at: 2026-01-12T16:14:14Z
```

# [complete] clap panics if generation is attemptet without having `bin_name` set for app and subcommands (non-descriptive errors)

---

_@hasezoey_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.0 (02072b482 2022-01-11)

### Clap Version

3.0.9

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};
#[derive(Parser)]
struct Main {
  #[clap(subcommand)]
  subcommands: SubCommands,
}

#[derive(Subcommand)]
enum SubCommands {
  Test(Sub),
}

#[derive(Parser)]
struct Sub {
  #[clap(short, long)]
  something: bool,
}

fn main() {
  clap_complete::Shell::Bash.generate(&Main::into_app()), &mut io::stdout());
}
```


### Steps to reproduce the bug with the above code

- `cargo run`

### Actual Behaviour

Panics (non descriptive) because `bin_name` is missing in `Main`, error something like:
```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /home/hasezoey/.local/cargo/registry/src/github.com-1ecc6299db9ec823/clap_complete-3.0.4/src/shells/bash.rs:17:43
```
^ the above could be improved with a `.expect` to show what is missing

when adding `#[clap(bin_name("something"))]` to `Main`, the next panic happens, because `bin_name` is also unset for the subcommand:
```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /home/hasezoey/.local/cargo/registry/src/github.com-1ecc6299db9ec823/clap_complete-3.0.4/src/generator/utils.rs:50:45
```
^ the above could be improved with a `.expect` to show what is missing

### Expected Behaviour

to show more descriptive errors (or not panic at all)

### Additional Context

why does the subcommand need to have `bin_name` set?

### Debug Output

_No response_

---

_Label `C-bug` added by @hasezoey on 2022-01-19 10:41_

---

_Comment by @epage on 2022-01-19 15:10_

This is failing because invariants need to be setup before this call.  While its calling out this invariant, there are many others that we are not detecting that are missing.  This is why we document using `clap_complete::generate` and `clap_complete::generate_to`.  See https://docs.rs/clap_complete/latest/clap_complete/

---

_Closed by @epage on 2022-01-19 15:10_

---

_Comment by @hasezoey on 2022-01-20 01:12_

> This is failing because invariants need to be setup before this call. While its calling out this invariant, there are many others that we are not detecting that are missing. This is why we document using `clap_complete::generate` and `clap_complete::generate_to`. See https://docs.rs/clap_complete/latest/clap_complete/

sorry, i dont completely understand what you are saying

my issue was actually 2 fold:
- a issue about improving errors (saying what is actually missing, like in this case `bin_name`)
- and a issue about why `bin_name` is required for subcommands

> This is failing because invariants need to be setup before this call

what `invariants` need to be setup before what call exactly?
(are you basically saying to use [`generate` (top-level)](https://docs.rs/clap_complete/latest/clap_complete/generator/fn.generate.html) instead of [`generate` (on `Shell`)](https://docs.rs/clap_complete/latest/clap_complete/enum.Shell.html#method.generate)?)

---

_Comment by @epage on 2022-01-20 02:44_

>  * a issue about improving errors (saying what is actually missing, like in this case bin_name)
> * and a issue about why bin_name is required for subcommands

The reason the error message isn't better is that it isn't intended for this case.  Same for why the bin_name is required

> (are you basically saying to use generate (top-level) instead of generate (on Shell)?)

Yes, like the example given in the documentation.

---

_Comment by @hasezoey on 2022-01-20 03:03_

> Yes, like the example given in the documentation.

then maybe it would be great to add a note to not use this function directly, i assumed i can use that function directly instead of the top-level to make it "more clean looking"

---

_Referenced in [clap-rs/clap#3322](../../clap-rs/clap/pulls/3322.md) on 2022-01-20 07:27_

---

_Comment by @hasezoey on 2022-01-20 07:28_

For now i made a PR to change the undescriptive `unwrap`'s to `expect`'s, see https://github.com/clap-rs/clap/pull/3322

---

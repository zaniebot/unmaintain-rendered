```yaml
number: 3864
title: "`ArgAction::Count` type error not caught at compile time"
type: issue
state: open
author: cyqsimon
labels:
  - C-bug
  - E-medium
  - E-help-wanted
  - A-derive
assignees: []
created_at: 2022-06-23T08:22:57Z
updated_at: 2023-01-07T00:21:48Z
url: https://github.com/clap-rs/clap/issues/3864
synced_at: 2026-01-12T16:14:15Z
```

# `ArgAction::Count` type error not caught at compile time

---

_@cyqsimon_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.61.0 (fe5b13d68 2022-05-18)

### Clap Version

v3.2.6

### Minimal reproducible code

```rust
use clap::{ArgAction, Parser};

#[derive(Debug, Clone, Parser)]
pub struct CliArgs {
    #[clap(short = 'v', action = ArgAction::Count)]
    pub verbose: i32,
}
fn main() {
    let _args = CliArgs::parse();
    println!("Hello, world!");
}
```

### Steps to reproduce the bug with the above code

 - Run `cargo build` and observe that the program builds.
 - Run `cargo run` and observe that the program panics at runtime due to a type error.

### Actual Behaviour

Type error causes a panic at runtime.

```
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `u8`,
 right: `i32`: Argument `verbose`'s selected action Count contradicts `value_parser` (ValueParser::other(i32))'
```

### Expected Behaviour

Type error should be caught at compile time.

Alternatively, allow upcasting the `u8` into user`s preferred type automatically.

---

_Label `C-bug` added by @cyqsimon on 2022-06-23 08:22_

---

_Label `A-derive` added by @epage on 2022-06-28 01:48_

---

_Label `S-waiting-on-design` added by @epage on 2022-06-28 01:48_

---

_Label `S-waiting-on-design` removed by @epage on 2022-06-28 01:49_

---

_Label `E-easy` added by @epage on 2022-06-28 01:49_

---

_Comment by @epage on 2022-06-28 01:50_

Thinking through this and the problems we normally have in moving an error to compile time, I think this is one case where we skip most of those.

---

_Label `E-easy` removed by @epage on 2022-06-28 01:50_

---

_Label `E-medium` added by @epage on 2022-06-28 01:50_

---

_Label `E-help-wanted` added by @epage on 2022-06-28 01:50_

---

_Comment by @slonik-az on 2022-10-24 01:42_

The better solution is to automatically downcast to user provided type.
Currently, one cannot even downcast `u8 -> usize`, see #4417 and #3912.


---

_Comment by @tgross35 on 2023-01-07 00:18_

I think this is also an issue in the example on the [args trait page](https://docs.rs/clap/latest/clap/trait.Args.html). Currently it has:

```rs
#[derive(clap::Parser)]
struct Args {
   #[command(flatten)]
   logging: LogArgs,
}

#[derive(clap::Args)]
struct LogArgs {
   #[arg(long, short = 'v', action = clap::ArgAction::Count)]
   verbose: i8,
}
```

And I had something based on this that didn't work until I changed `i8` to `u8`

Edit: submitted #4610 to fix just this example

---

_Referenced in [clap-rs/clap#4610](../../clap-rs/clap/pulls/4610.md) on 2023-01-07 00:21_

---

_Referenced in [clap-rs/clap#4830](../../clap-rs/clap/issues/4830.md) on 2023-04-10 20:46_

---

_Referenced in [clap-rs/clap#4467](../../clap-rs/clap/issues/4467.md) on 2023-05-08 11:55_

---

_Referenced in [clap-rs/clap#2419](../../clap-rs/clap/issues/2419.md) on 2023-06-15 19:27_

---

_Referenced in [clap-rs/clap#5135](../../clap-rs/clap/issues/5135.md) on 2023-09-25 11:59_

---

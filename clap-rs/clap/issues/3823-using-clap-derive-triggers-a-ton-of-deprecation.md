```yaml
number: 3823
title: Using clap derive triggers a ton of deprecation warnings since 3.2
type: issue
state: closed
author: Ten0
labels:
  - C-bug
  - S-duplicate
assignees: []
created_at: 2022-06-13T16:15:22Z
updated_at: 2022-06-13T16:27:28Z
url: https://github.com/clap-rs/clap/issues/3823
synced_at: 2026-01-12T16:14:15Z
```

# Using clap derive triggers a ton of deprecation warnings since 3.2

---

_@Ten0_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

Latest stable (1.61)

### Clap Version

3.2.1

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
pub struct Args {
    /// Input delimiter
    #[structopt(short, long, default_value = ",")]
    pub delimiter: String,
    #[structopt(short, long)]
    pub flexible: bool,
    /// Abc
    #[structopt(short, long = "abece")]
    pub abc: Vec<String>,
}

```


### Steps to reproduce the bug with the above code

```bash
cargo update
cargo build
```

### Actual Behaviour

```
warning: use of deprecated unit variant `clap::ArgAction::StoreValue`: Replaced with `ArgAction::Set` or `ArgAction::Append`
 --> src/lib.rs:5:5
  |
5 |     /// Input delimiter
  |     ^^^^^^^^^^^^^^^^^^^
  |
  = note: `#[warn(deprecated)]` on by default

warning: use of deprecated unit variant `clap::ArgAction::StoreValue`: Replaced with `ArgAction::Set` or `ArgAction::Append`
  --> src/lib.rs:10:5
   |
10 |     /// Abc
   |     ^^^^^^^

warning: use of deprecated associated function `clap::ArgMatches::is_present`: Replaced with either `ArgAction::SetTrue` or `ArgMatches::contains_id(...)`
 --> src/lib.rs:9:19
  |
9 |     pub flexible: bool,
  |                   ^^^^

warning: use of deprecated associated function `clap::Arg::<'help>::validator`: Replaced with `Arg::value_parser(...)`
 --> src/lib.rs:5:5
  |
5 |     /// Input delimiter
  |     ^^^^^^^^^^^^^^^^^^^

warning: use of deprecated associated function `clap::Arg::<'help>::multiple_occurrences`: Replaced with `Arg::action` (Issue #3772)
  --> src/lib.rs:12:14
   |
12 |     pub abc: Vec<String>,
   |              ^^^

warning: use of deprecated associated function `clap::Arg::<'help>::validator`: Replaced with `Arg::value_parser(...)`
  --> src/lib.rs:10:5
   |
10 |     /// Abc
   |     ^^^^^^^

```

### Expected Behaviour

*no warnings*

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Ten0 on 2022-06-13 16:15_

---

_Renamed from "Using clap derive triggers a ton of deprecation warnings" to "Using clap derive triggers a ton of deprecation warnings since 3.2" by @Ten0 on 2022-06-13 16:15_

---

_Comment by @epage on 2022-06-13 16:16_

This is a duplicate of #3820

---

_Closed by @epage on 2022-06-13 16:16_

---

_Label `S-duplicate` added by @epage on 2022-06-13 16:17_

---

_Comment by @Ten0 on 2022-06-13 16:18_

> This is a duplicate of #3820

That doesn't seem related in any way to the #3820 PR which fixes a panic

---

_Comment by @Ten0 on 2022-06-13 16:20_

In addition there's no other open issue since 3.2 was released and introduced this issue a few hours ago, so I don't see how this can be a duplicate of anything.

---

_Comment by @epage on 2022-06-13 16:21_

Was off by 2, it is #3822

---

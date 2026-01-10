---
number: 4202
title: "`clap-derive`: error message does not match usage of `default_value_t`"
type: issue
state: closed
author: bengsparks
labels:
  - C-bug
assignees: []
created_at: 2022-09-10T17:09:18Z
updated_at: 2022-09-10T21:24:12Z
url: https://github.com/clap-rs/clap/issues/4202
synced_at: 2026-01-10T01:27:51Z
---

# `clap-derive`: error message does not match usage of `default_value_t`

---

_Issue opened by @bengsparks on 2022-09-10 17:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.59.0-nightly (efec54529 2021-12-04)

### Clap Version

3.2.20

### Minimal reproducible code

```
use clap::Parser;

#[derive(Parser, Debug)]
struct Args {
    #[clap(default_value_t = false)]
    g: Option<bool>,
}

fn main() {
    println!("Hello, world!");
}
```

### Steps to reproduce the bug with the above code

`cargo build`

### Actual Behaviour

Correctly fails to compile, but the provided message is not quite correct.

```rust
error: default_value is meaningless for Option
 --> src/main.rs:5:12
  |
5 |     #[clap(default_value_t = false)]
```

### Expected Behaviour

the `default_value` from the error should be `default_value_t`:

```
error: default_value_t is meaningless for Option
 --> src/main.rs:5:12
  |
5 |     #[clap(default_value_t = false)]
```

### Additional Context

_No response_

### Debug Output

Identical:

```rust
error: default_value is meaningless for Option
 --> src/main.rs:5:12
  |
5 |     #[clap(default_value_t = false)]
  |   
```

---

_Label `C-bug` added by @bengsparks on 2022-09-10 17:09_

---

_Renamed from "Error message does not match usage" to "`clap-derive`: error message does not match usage of `default_value_t`" by @bengsparks on 2022-09-10 17:10_

---

_Comment by @epage on 2022-09-10 21:24_

I've actually removed this error message in the upcoming clap v4

---

_Closed by @epage on 2022-09-10 21:24_

---

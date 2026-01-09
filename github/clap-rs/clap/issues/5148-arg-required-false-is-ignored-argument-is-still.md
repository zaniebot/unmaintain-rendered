---
number: 5148
title: "`arg(required = false)` is ignored (argument is still required)"
type: issue
state: closed
author: Andrew15-5
labels:
  - C-bug
assignees: []
created_at: 2023-09-30T12:21:05Z
updated_at: 2023-09-30T13:33:23Z
url: https://github.com/clap-rs/clap/issues/5148
synced_at: 2026-01-07T13:12:20-06:00
---

# `arg(required = false)` is ignored (argument is still required)

---

_Issue opened by @Andrew15-5 on 2023-09-30 12:21_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.1 (d5c2e9c34 2023-09-13)

### Clap Version

4.4.6

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct Args {
    #[arg(required = false)]
    output: std::path::PathBuf,
}

fn main() {
    Args::parse();
}
```

### Steps to reproduce the bug with the above code

```sh
cargo new program
cd program
cargo add clap -F derive
# Edit ./src/main.rs
cargo run
```

### Actual Behaviour

Exists with status code 2:

```
error: The following required argument was not provided: output
                     ^^^^^^^^ Says that is is somehow required.
Usage: program [OUTPUT] <- Literally says that is is optional.

For more information, try '--help'.
```

### Expected Behaviour

Status code 0 and no stderr or stdout data.

### Additional Context

I haven't found anywhere about specifically `#[arg(required = true)]` in https://docs.rs/clap/latest/clap/_derive/index.html, but after seeing `Arg::required(false)` and `#[group(required = true)]` I tried, and it did change the help message. However, I can use the program without passing the value to the output optional argument.

### Debug Output

```toml
[dependencies]
clap = { version = "4.4.6", features = ["derive"] }
```

---

_Label `C-bug` added by @Andrew15-5 on 2023-09-30 12:21_

---

_Comment by @epage on 2023-09-30 13:08_

The error isn't coming from clap's validation but from the derive trying to fill in your struct.

Instead of marking it `required = false`, wrap the type in `Option`

---

_Closed by @epage on 2023-09-30 13:08_

---

_Comment by @Andrew15-5 on 2023-09-30 13:33_

Ooh, so `Option` is literally the `Option` enum, and it acts like an _optional_ CLI argument. I thought that this is not related. Thanks. Here is the related table: https://docs.rs/clap/latest/clap/_derive/index.html#arg-types.

---

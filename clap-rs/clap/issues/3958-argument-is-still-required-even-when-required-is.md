---
number: 3958
title: Argument is still required even when required is set to false in derive macro.
type: issue
state: closed
author: Atsukoro1
labels:
  - C-bug
assignees: []
created_at: 2022-07-19T21:44:11Z
updated_at: 2022-07-19T21:52:04Z
url: https://github.com/clap-rs/clap/issues/3958
synced_at: 2026-01-10T01:27:49Z
---

# Argument is still required even when required is set to false in derive macro.

---

_Issue opened by @Atsukoro1 on 2022-07-19 21:44_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

3.2.12

### Minimal reproducible code
```rust
use std::{
    process,
};
use clap::Parser;

/// Command line utility to analyze packets flowing through your device
#[derive(Parser, Debug)]
#[clap(
    version, 
    about, 
    long_about = None
)]
pub struct Args {
    /// Count of packets to print
    #[clap(short, long, value_parser, default_value_t = 1000, required = false)]
    count: u128,

    /// Set the action to sniffing packets
    #[clap(short, long, value_parser, default_value = "false", required = false)]
    sniff: bool,

    /// Interface to capture packets on
    #[clap(short, long, value_parser, takes_value = false, required = true)]
    interface: String,

    /// Filter values only coming from specified ip address
    #[clap(short, long, value_parser, default_value = "all", required = false)]
    from: String,

    /// Filter values only coming from this specified address
    #[clap(short, long, value_parser, default_value = "all", required = false)]
    to: String
}

#[tokio::main]
async fn main() -> process::ExitCode {
    Args::parse();

    process::exit(0);
}
```

### Steps to reproduce the bug with the above code

cargo run main.rs

### Actual Behaviour

Interface field is always required even when it's set to not be.

### Expected Behaviour

The interface field shouldn't be required.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @Atsukoro1 on 2022-07-19 21:44_

---

_Comment by @epage on 2022-07-19 21:52_

`interface` is a `String` without a `default_value` being set.  When creating the struct, clap doesn't know what to fill the `interface` field with, so it is erroring. 

Side note: generally, you shouldn't need to set the `required` attribute.  clap will automatically set `required = false` when a default value is provided or when the type is wrapped in `Option<T>`.

---

_Closed by @epage on 2022-07-19 21:52_

---

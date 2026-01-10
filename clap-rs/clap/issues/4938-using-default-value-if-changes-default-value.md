---
number: 4938
title: "Using `default_value_if` changes default value"
type: issue
state: closed
author: sw0x2A
labels:
  - C-bug
assignees: []
created_at: 2023-05-24T18:59:58Z
updated_at: 2023-05-24T21:19:19Z
url: https://github.com/clap-rs/clap/issues/4938
synced_at: 2026-01-10T01:28:03Z
---

# Using `default_value_if` changes default value

---

_Issue opened by @sw0x2A on 2023-05-24 18:59_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0 (84c898d65 2023-04-16)

### Clap Version

clap = { version = "4.3.0", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::{builder::ArgPredicate, Parser};

#[derive(Parser, PartialEq, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    #[arg(long)]
    bar: bool,

    #[arg(long, default_value_if("bar", ArgPredicate::IsPresent, Some("true")))]
    foo: bool,
}

#[derive(Parser, PartialEq, Debug)]
#[command(author, version, about, long_about = None)]
struct Args2 {
    #[arg(long)]
    bar: bool,

    #[arg(long)]
    foo: bool,
}

fn main() {
    let args = Args::parse;
}

#[test]
fn test_clap_verify_args() {
    use clap::CommandFactory;
    Args::command().debug_assert();
}

#[test]
fn test_clap_defaults() {
    let args = Args::parse_from(["bla"]);
    assert_eq!(args, Args {
        bar: false,
        foo: false,
    });
}

#[test]
fn test_clap_healthcheck() {
    let args = Args::parse_from(["bla", "--bar"]);
    assert_eq!(args, Args {
        bar: true,
        foo: true,
    });
}

#[test]
fn test_clap_defaults2() {
    let args = Args2::parse_from(["bla"]);
    assert_eq!(args, Args2 {
        bar: false,
        foo: false,
    });
}
```


### Steps to reproduce the bug with the above code

Run `cargo test`.



### Actual Behaviour

Field `foo` of `Args` is true when `default_value_if` is used even though its condition is not fulfilled.
`Args2`, that is equal to `Args` except that it is without `default_value_if` on field `foo`, shows correct, expected behaviour.

Maybe related to #4918.

### Expected Behaviour

Default value of `foo` in `Args` should be `false` when `bar` is not present.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @sw0x2A on 2023-05-24 18:59_

---

_Comment by @epage on 2023-05-24 21:19_

Yes, this looks like #4918, closing in favor of that

---

_Closed by @epage on 2023-05-24 21:19_

---

---
number: 3514
title: "[3.1] Derive parsing FromStr args into a vector regression from 3.0"
type: issue
state: closed
author: ufoscout
labels:
  - C-bug
  - A-builder
  - E-easy
assignees: []
created_at: 2022-02-26T13:41:07Z
updated_at: 2022-02-28T16:04:13Z
url: https://github.com/clap-rs/clap/issues/3514
synced_at: 2026-01-07T13:12:19-06:00
---

# [3.1] Derive parsing FromStr args into a vector regression from 3.0

---

_Issue opened by @ufoscout on 2022-02-26 13:41_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.59.0 (9d1b2106e 2022-02-23)

### Clap Version

3.1.2

### Minimal reproducible code

```rust
use clap::Parser;
use std::str::FromStr;

#[derive(Debug, PartialEq)]
pub enum MyType {
    A,
    B,
}

impl FromStr for MyType {
    type Err = String;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        match s.to_lowercase().as_str() {
            "a" => Ok(MyType::A),
            "b" => Ok(MyType::B),
            _ => Err("".to_owned()),
        }
    }
}

#[derive(Parser)]
pub struct Config {
    #[clap(long, default_value = "a,b,a", value_delimiter = ',')]
    pub values: Vec<MyType>,
}

// These two tests pass in 3.0 but fail in 3.1

#[test]
fn shuold_build_a_vec_of_types() {
    let args: Vec<std::ffi::OsString> = vec!["".into()];
    let config = Config::parse_from(args);
    assert_eq!(vec![MyType::A, MyType::B, MyType::A], config.values);
}

#[test]
fn shuold_build_a_vec_of_types_from_default_value() {
    let args: Vec<std::ffi::OsString> = vec!["".into(), "--values=a,b".into()];
    let config = Config::parse_from(args);
    assert_eq!(vec![MyType::A, MyType::B], config.values);
}
```


### Steps to reproduce the bug with the above code

Executes the two tests provided in the minimal reproducible code; they both fail in 3.1 but pass in 3.0

### Actual Behaviour

A validation error is thrown when the configuration is built.

### Expected Behaviour

The configuration is valid in 3.0 and should be valid in 3.1 too.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ufoscout on 2022-02-26 13:41_

---

_Label `A-builder` added by @epage on 2022-02-26 18:03_

---

_Label `E-easy` added by @epage on 2022-02-26 18:03_

---

_Comment by @epage on 2022-02-26 18:18_

The error message
> thread 'main' panicked at 'Argument `values`'s default_value=a,b,a failed validation: ', /home/epage/src/personal/clap/src/build/debug_asserts.rs:690:21
> note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

This regression was introduced in #3423 because delimited values were not taken into account.

To fix this, we need to take delimited values into account and add tests for this case (ideally at the builder level).

---

_Referenced in [clap-rs/clap#3521](../../clap-rs/clap/pulls/3521.md) on 2022-02-28 15:22_

---

_Closed by @epage on 2022-02-28 15:55_

---

_Comment by @epage on 2022-02-28 16:04_

v3.1.3 is now released with a fix for this.

---

_Referenced in [clap-rs/clap#3541](../../clap-rs/clap/pulls/3541.md) on 2022-03-07 03:58_

---

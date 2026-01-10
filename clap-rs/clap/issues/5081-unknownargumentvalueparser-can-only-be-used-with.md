---
number: 5081
title: "`UnknownArgumentValueParser` can only be used with string value argument"
type: issue
state: open
author: weihanglo
labels:
  - C-bug
  - M-breaking-change
  - A-builder
assignees: []
created_at: 2023-08-19T09:40:09Z
updated_at: 2023-08-21T16:27:52Z
url: https://github.com/clap-rs/clap/issues/5081
synced_at: 2026-01-10T01:28:06Z
---

# `UnknownArgumentValueParser` can only be used with string value argument

---

_Issue opened by @weihanglo on 2023-08-19 09:40_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.0-beta.11 (7a3a43a3b 2023-08-18)

### Clap Version

4.3.23

### Minimal reproducible code

```rust
#!/usr/bin/env -S cargo +nightly -Zscript

//! ```cargo
//! [dependencies]
//! clap = "=4.3.23"
//! ```

fn main() {
    let args = clap::Command::new("test")
        .args([clap::Arg::new("libtest-ignore")
            .long("ignored")
            .action(clap::ArgAction::SetTrue)
            .value_parser(
                clap::builder::UnknownArgumentValueParser::suggest_arg("-- --ignored")
                    .and_suggest("not much else to say"),
            )
            .hide(true)])
        .get_matches();

    assert!(matches!(
        args.try_get_one::<bool>("libtest-ignore"),
        Err(clap::parser::MatchesError::Downcast { .. })
    ));

    args.try_get_one::<bool>("libtest-ignore").unwrap();
    // thread 'main' panicked at run.rs:25:48:
    // called `Result::unwrap()` on an `Err` value: Downcast { actual: alloc::string::String, expected: bool }
    // note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
}
```


### Steps to reproduce the bug with the above code

Run the script above.

### Actual Behaviour

I am not sure if this is expected. It turns out that the unknown argument must be defined as `--flag <string>`. Might be the result of `StringValueParser` in use.

Here is the current cargo integration that failed <https://github.com/rust-lang/cargo/pull/12529>.

`keep-going` is [always evaluated](https://github.com/rust-lang/cargo/blob/80eca0e58fb2ff52c1e94fc191b55b37ed73e0e4/src/cargo/util/command_prelude.rs#L586) when constructing `BuildConfig` even it is not a valid argument. Cargo [provides a default value for unknown argument](https://github.com/rust-lang/cargo/blob/80eca0e58fb2ff52c1e94fc191b55b37ed73e0e4/src/cargo/util/command_prelude.rs#L794-L798), which contradicts the definition of [bool `--keep-going`](https://github.com/rust-lang/cargo/blob/80eca0e58fb2ff52c1e94fc191b55b37ed73e0e4/src/cargo/util/command_prelude.rs#L433-L435).

### Expected Behaviour

`UnknownArgumentValueParser` can use with any type of argument.

### Additional Context

cc #5080

### Debug Output

_No response_

---

_Label `C-bug` added by @weihanglo on 2023-08-19 09:40_

---

_Referenced in [rust-lang/cargo#12529](../../rust-lang/cargo/pulls/12529.md) on 2023-08-19 09:40_

---

_Comment by @epage on 2023-08-21 16:22_

I had considered making the it parametrized but thought that might be too complex.

Maybe with a defaulted generic argument it won't be too bad and likely won't be a breaking change.

Alternatively, cargo could ignore the "invalid type" error since its pattern of reuse is a bit weird in of itself.

---

_Comment by @epage on 2023-08-21 16:27_

Even though in some circles, a new defaulted generic parameter is not a breaking change, for how `UnknownArgumentValueParser` is used, it is a breaking change so we can't resolve this until clap v5.

---

_Label `M-breaking-change` added by @epage on 2023-08-21 16:27_

---

_Label `A-builder` added by @epage on 2023-08-21 16:27_

---

_Added to milestone `5.0` by @epage on 2023-08-21 16:27_

---

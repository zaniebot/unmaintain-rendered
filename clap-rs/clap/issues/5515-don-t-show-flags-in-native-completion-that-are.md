---
number: 5515
title: "Don't show flags in native completion that are already present"
type: issue
state: open
author: HKalbasi
labels:
  - C-bug
  - E-hard
  - A-completion
assignees: []
created_at: 2024-06-01T17:48:03Z
updated_at: 2024-10-23T01:56:26Z
url: https://github.com/clap-rs/clap/issues/5515
synced_at: 2026-01-10T01:28:13Z
---

# Don't show flags in native completion that are already present

---

_Issue opened by @HKalbasi on 2024-06-01 17:48_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.76

### Clap Version

4.5.2

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser, ValueEnum};

#[derive(Debug, Parser)]
#[command(version, about, long_about = None)]
struct Cli {
    /// What mode to run the program in
    #[arg(short, long, value_enum)]
    mode: Mode,
}

#[derive(Debug, Copy, Clone, PartialEq, Eq, PartialOrd, Ord, ValueEnum)]
enum Mode {
    /// Run swiftly
    Fast,
    /// Crawl slowly but steadily
    ///
    /// This paragraph is ignored because there is no long help text for possible values.
    Slow,
}
fn main() {
    let mut cmd = Cli::command();
    let args = std::env::args_os().collect::<Vec<_>>();
    let arg_index = args.len() - 1;
    let r = clap_complete::dynamic::complete(&mut cmd, args, arg_index, None).unwrap();
    dbg!(r);
}}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --mode fast --mo
```

### Actual Behaviour

```
[src/main.rs:25:5] r = [
    (
        "--mode",
        Some(
            StyledStr(
                "What mode to run the program in",
            ),
        ),
    ),
]
```

### Expected Behaviour

```
[src/main.rs:25:5] r = []
```

### Additional Context

Using `--mode` is invalid, so we should not complete it. Obviously flags that accept multiple values should keep the existing behavior.

### Debug Output

_No response_

---

_Label `C-bug` added by @HKalbasi on 2024-06-01 17:48_

---

_Label `A-completion` added by @epage on 2024-06-02 01:02_

---

_Label `E-easy` added by @epage on 2024-06-02 01:02_

---

_Comment by @epage on 2024-06-02 01:03_

There is a little more to it. We should only do this if the ArgAction prevenhs it and if we don't override ourself.

---

_Referenced in [clap-rs/clap#5784](../../clap-rs/clap/issues/5784.md) on 2024-10-21 23:57_

---

_Label `E-easy` removed by @epage on 2024-10-23 01:54_

---

_Label `E-hard` added by @epage on 2024-10-23 01:54_

---

_Comment by @epage on 2024-10-23 01:56_

I changed the difficulty of this is I'm a bit unsure how much we need to get into correctness of the parser to track this state.  Currently, we are pretty sloppy which we can get away with because its fine to generate more options than reasonable.  If we start removing options, we'll need to be more accurate.

We might even want to block this on other efforts, like supporting for `allow_hyphen_values` and other settings that deal with correctness.

---

_Referenced in [clap-rs/clap#5884](../../clap-rs/clap/pulls/5884.md) on 2025-01-20 15:31_

---

---
number: 6130
title: "Completion keeps suggesting options (long/short) after an escape (`--`)"
type: issue
state: closed
author: mernen
labels:
  - C-bug
assignees: []
created_at: 2025-09-13T23:03:40Z
updated_at: 2025-09-16T01:36:25Z
url: https://github.com/clap-rs/clap/issues/6130
synced_at: 2026-01-10T01:28:22Z
---

# Completion keeps suggesting options (long/short) after an escape (`--`)

---

_Issue opened by @mernen on 2025-09-13 23:03_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.89.0 (29483883e 2025-08-04)

### Clap Version

4.5.43

### Minimal reproducible code

```rust
use clap::{Arg, Command, value_parser};

fn main() {
    let mut cmd = Command::new("example").args([
        Arg::new("complete")
            .long("complete")
            .value_name("SHELL")
            .value_parser(value_parser!(clap_complete::Shell))
            .help("complete"),
        Arg::new("mode")
            .num_args(0..)
            .value_name("MODE")
            .value_parser(["read", "write"]),
    ]);

    let matches = cmd.get_matches_mut();
    if let Some(generator) = matches.get_one::<clap_complete::Shell>("complete") {
        let bin_name = std::env::args()
            .next()
            .unwrap_or_else(|| cmd.get_name().to_string());
        clap_complete::generate(*generator, &mut cmd, bin_name, &mut std::io::stdout());
    } else {
        println!("{matches:?}");
    }
}
```


### Steps to reproduce the bug with the above code

1. Generate completions, e.g.: `. <(cargo run -- --complete "$SHELL")`
2. Enter a command that has a `--` escape, e.g.: `target/debug/example -- `
3. Press Tab to trigger completion


### Actual Behaviour

Options are suggested along positional values:

    -h          --complete  --help      read        write

### Expected Behaviour

If the input contains a `--` argument, options are no longer parsed, so they shouldn't be suggested. Completion suggestions should only be:

    read        write

### Additional Context

This occurs at least with bash and the dynamic completions; I'm not sure about other shells.

### Debug Output

_No response_

---

_Label `C-bug` added by @mernen on 2025-09-13 23:03_

---

_Referenced in [clap-rs/clap#6131](../../clap-rs/clap/pulls/6131.md) on 2025-09-13 23:04_

---

_Closed by @epage on 2025-09-16 01:36_

---

---
number: 5498
title: "Clap complete dynamic doesn't work with `ValueEnum` in args"
type: issue
state: closed
author: HKalbasi
labels:
  - C-bug
assignees: []
created_at: 2024-05-22T22:44:33Z
updated_at: 2024-05-22T22:58:12Z
url: https://github.com/clap-rs/clap/issues/5498
synced_at: 2026-01-07T13:12:20-06:00
---

# Clap complete dynamic doesn't work with `ValueEnum` in args

---

_Issue opened by @HKalbasi on 2024-05-22 22:44_

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
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --mode fa
```

### Actual Behaviour

The output is:
```
[src/main.rs:25:5] r = []
```

### Expected Behaviour

The output should be:
```
[src/main.rs:25:5] r = [
    (
        "fast",
        Some(
            StyledStr(
                "Run swiftly",
            ),
        ),
    ),
]
```

### Additional Context

_No response_

### Debug Output

```
[clap_builder::builder::command]Command::_build: name="clap-test"
[clap_builder::builder::command]Command::_propagate:clap-test
[clap_builder::builder::command]Command::_check_help_and_version:clap-test expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:clap-test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:mode
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["mode"]
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"mode" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("\u{1b}[1m--mode\u{1b}[0m <MODE>")]
[clap_complete::dynamic::completer]     complete::next: Begin parsing '"--mode"'
[clap_complete::dynamic::completer]     complete_arg: arg=ParsedArg { inner: "fa" }, cmd="clap-test", current_dir=None, pos_index=1, is_escaped=false
[clap_complete::dynamic::completer]     complete_subcommand: cmd="clap-test", value="fa"
[clap_complete::dynamic::completer]     subcommands: name=clap-test
[clap_complete::dynamic::completer]     subcommands: Has subcommands...false
```

---

_Label `C-bug` added by @HKalbasi on 2024-05-22 22:44_

---

_Comment by @HKalbasi on 2024-05-22 22:58_

Ah, this is duplicate of #3920, sorry for noise.

---

_Closed by @HKalbasi on 2024-05-22 22:58_

---

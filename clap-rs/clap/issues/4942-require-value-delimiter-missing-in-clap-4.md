```yaml
number: 4942
title: require_value_delimiter missing in Clap-4
type: issue
state: closed
author: asomers
labels:
  - C-bug
assignees: []
created_at: 2023-05-27T00:15:42Z
updated_at: 2023-05-27T19:07:14Z
url: https://github.com/clap-rs/clap/issues/4942
synced_at: 2026-01-12T16:14:16Z
```

# require_value_delimiter missing in Clap-4

---

_@asomers_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.0-nightly (77f4f828a 2023-05-20)

### Clap Version

4.3.0

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
struct Cli {
    #[clap(
        // How to specify require_value_delimiter?  It was removed in Clap-4.0.0
        // require_value_delimiter(true),
        value_delimiter(',')
    )]
    options: Vec<i32>,
    #[clap(num_args(1..), required(true))]
    pargs: Vec<String>
}

fn main() {
    let cli = Cli::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run` without arguments will fail with the below error.

### Actual Behaviour

Fails with an error like this:
```
thread 'main' panicked at 'Only one positional argument with `.num_args(1..)` set is allowed per command, unless the second one also has .last(true) set', /playground/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_builder-4.2.7/src/builder/debug_asserts.rs:615:9
```

### Expected Behaviour

The program should run fine.  And it should accept invocations like:
```
cargo run -- option1 parg1
cargo run -- option1,option2 parg1
cargo run -- option1,option2 parg1 parg2
```

### Additional Context

The goal is to have two required Args, one of which is comma-delimited and the other space-delimited.  Similar to the `zfs get` command.

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-require-value-delimiter"
[clap_builder::builder::command]Command::_propagate:clap-require-value-delimiter
[clap_builder::builder::command]Command::_check_help_and_version:clap-require-value-delimiter expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-require-value-delimiter
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:options
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:pargs
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
```

---

_Label `C-bug` added by @asomers on 2023-05-27 00:15_

---

_Comment by @epage on 2023-05-27 01:38_

What you most likely want is to just disable some of the automatic `Vec` behavior:
```rust
use clap::Parser;

#[derive(Parser, Debug)]
struct Cli {
    #[clap(
        num_args = 1,
        action = ArgAction::Set,
        value_delimiter(',')
    )]
    options: Vec<i32>,
    #[clap(num_args(1..), required(true))]
    pargs: Vec<String>
}

fn main() {
    let cli = Cli::parse();
}
```

---

_Comment by @asomers on 2023-05-27 19:07_

Thanks!  It would've taken me forever to figure that out by myself.  FTR, I also had to add a `required(true)`.  So the complete program is
```rust
use clap::{ArgAction, Parser};

#[derive(Parser, Debug)]
struct Cli {
    #[clap(
        num_args = 1,
        action = ArgAction::Set,
        required(true),
        value_delimiter(',')
    )]
    options: Vec<i32>,
    #[clap(num_args(1..), required(true))]
    pargs: Vec<String>
}

fn main() {
    let cli = Cli::parse();
    dbg!(&cli.options, &cli.pargs);
}
```

---

_Closed by @asomers on 2023-05-27 19:07_

---

_Referenced in [neherlab/pangraph#120](../../neherlab/pangraph/pulls/120.md) on 2025-02-04 14:50_

---

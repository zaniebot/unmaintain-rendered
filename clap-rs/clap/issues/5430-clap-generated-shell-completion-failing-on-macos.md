```yaml
number: 5430
title: "Clap-generated shell completion failing on macOS /bin/bash with: conditional binary operator expected: syntax error near `IFS'"
type: issue
state: closed
author: gibfahn
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2024-03-26T16:18:27Z
updated_at: 2024-04-17T12:35:00Z
url: https://github.com/clap-rs/clap/issues/5430
synced_at: 2026-01-12T16:14:17Z
```

# Clap-generated shell completion failing on macOS /bin/bash with: conditional binary operator expected: syntax error near `IFS'

---

_@gibfahn_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

```console
❯ rustc -V
rustc 1.77.0 (aedd173a2 2024-03-17)
```

### Clap Version

4.5.3

### Minimal reproducible code

```toml
# Cargo.toml
[package]
name = "my-app"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = { version = "4.5.3", features = [
  "debug",
  "derive",
  "env",
  "string",
  "wrap_help",
] }
clap_complete = "=4.5.1"
```

````rust
// src/main.rs
use clap::{CommandFactory, Parser, ValueHint};
use clap_complete::{generate, Shell};
use std::path::PathBuf;

#[derive(Parser)]
struct Opt {
    #[arg(short, long, value_hint = ValueHint::FilePath)]
    file: Option<PathBuf>,
}

fn main() {
    generate(
        Shell::Bash,
        &mut Opt::command(),
        "my-app",
        &mut std::io::stdout(),
    );
}
````


### Steps to reproduce the bug with the above code

(on a Mac)

```shell
cargo run | /bin/bash
```

### Actual Behaviour

Output:

```console
❯ cargo run | /bin/bash
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/my-app`
/bin/bash: line 30: conditional binary operator expected
/bin/bash: line 30: syntax error near `IFS'
/bin/bash: line 30: `                    if [[ -v IFS ]]; then'
```

### Expected Behaviour

Output:

```console
❯ cargo run | /bin/bash
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/my-app`
```

### Additional Context

On clap_complete 4.4.9 this works, on 4.4.10 this fails. I _think_ that this is a regression introduced by https://github.com/clap-rs/clap/pull/5336 . So the issue is probably the same as https://github.com/clap-rs/clap/issues/5190 , which was fixed by https://github.com/clap-rs/clap/pull/5278.

### Debug Output

```
[clap_builder::builder::command]Command::_build: name="my-app"
[clap_builder::builder::command]Command::_propagate:my-app
[clap_builder::builder::command]Command::_check_help_and_version:my-app expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:my-app
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:file
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
```

---

_Label `C-bug` added by @gibfahn on 2024-03-26 16:18_

---

_Comment by @epage on 2024-03-26 16:29_

This is happening too often and I'm tempted to say bash 4 is unsupported until #3166 is the default completion system since that at least reduces the risk we'll run into issues.

---

_Label `A-completion` added by @epage on 2024-03-26 16:29_

---

_Label `E-medium` added by @epage on 2024-03-26 16:30_

---

_Referenced in [clap-rs/clap#5444](../../clap-rs/clap/pulls/5444.md) on 2024-04-07 14:25_

---

_Comment by @sudotac on 2024-04-07 14:52_

Sorry for the inconvenience.
I thought `-v` was already supported on bash 4.0, but actually only available on 5.0 or later.
I submitted #5444 to fix this. Feel free to use it.

> This is happening too often and I'm tempted to say bash 4 is unsupported until https://github.com/clap-rs/clap/issues/3166 is the default completion system since that at least reduces the risk we'll run into issues.

I agree with it.
This time it's easy to fix that, but caring about unsupported features on (very) older versions anytime is generally a hard work.

(By the way, I'm personally looking forward to native completion system!)

---

_Closed by @epage on 2024-04-09 17:06_

---

_Comment by @gibfahn on 2024-04-17 12:34_

Thanks for fixing this!

---

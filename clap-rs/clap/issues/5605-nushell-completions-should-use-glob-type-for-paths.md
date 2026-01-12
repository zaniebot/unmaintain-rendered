```yaml
number: 5605
title: "Nushell completions should use `glob` type for paths"
type: issue
state: closed
author: kaathewisegit
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2024-07-28T18:55:25Z
updated_at: 2025-01-02T14:22:29Z
url: https://github.com/clap-rs/clap/issues/5605
synced_at: 2026-01-12T16:14:17Z
```

# Nushell completions should use `glob` type for paths

---

_@kaathewisegit_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.0 (051478957 2024-07-21) (Alpine Linux 1.80.0-r0)

### Clap Version

4.4.6 (4.5.3 for `clap_complete_nushell`)

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser};
use clap_complete::generate;
use clap_complete_nushell::Nushell;

use std::path::PathBuf;

#[derive(Parser)]
pub struct Options {
    path: PathBuf,

    rest: Vec<PathBuf>,
}

fn main() {
    let mut cmd = Options::command();
    generate(Nushell, &mut cmd, "sd", &mut std::io::stdout());
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The following completion is generated:

```Nushell
module completions {

  export extern sd [
    path: string
    ...rest: string
    --help(-h)                # Print help
  ]

}

export use completions *
```

### Expected Behaviour

- Arguments with type `PathBuf` should have the `path` type in the completions, so that the native path completion works.
- Arguments with type `Vec<PathBuf>` should have the `glob` type, otherwise globbing breaks: `sd a *` will return an error, because path `*` doesn't exist. 

### Additional Context

_No response_

### Debug Output

```
[clap_builder::builder::command]Command::_build: name="compl"
[clap_builder::builder::command]Command::_propagate:compl
[clap_builder::builder::command]Command::_check_help_and_version:compl expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:compl
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:path
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:rest
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=["path"]
[ clap_builder::output::usage]Usage::get_required_usage_from:iter:"path" arg is_present=false
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[StyledStr("<PATH>")]
```

---

_Label `C-bug` added by @kaathewisegit on 2024-07-28 18:55_

---

_Label `C-bug` removed by @epage on 2024-07-29 13:26_

---

_Label `C-enhancement` added by @epage on 2024-07-29 13:26_

---

_Label `A-completion` added by @epage on 2024-07-29 13:26_

---

_Comment by @epage on 2024-07-29 13:28_

`clap_complete_nushell` does not yet have support for [ValueHint](https://docs.rs/clap/latest/clap/enum.ValueHint.html).  Someone is welcome to contribute it and/or add Rust-native nushell completion support (see also #3166).

Note: we implicitly set the `ValueHint` based on an `Arg` being a `PathBuf`, see
https://github.com/clap-rs/clap/blob/16fba4b9f91cf772ab1f6e7b1016a257e5f6cf62/clap_builder/src/builder/arg.rs#L4023-L4037

---

_Referenced in [clap-rs/clap#5865](../../clap-rs/clap/pulls/5865.md) on 2025-01-02 02:54_

---

_Closed by @epage on 2025-01-02 14:22_

---

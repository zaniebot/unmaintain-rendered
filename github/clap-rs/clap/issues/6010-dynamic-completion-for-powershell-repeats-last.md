---
number: 6010
title: Dynamic completion for powershell repeats last word when completing next argument
type: issue
state: closed
author: jakobhellermann
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2025-05-22T20:00:17Z
updated_at: 2025-10-29T22:35:39Z
url: https://github.com/clap-rs/clap/issues/6010
synced_at: 2026-01-07T13:12:20-06:00
---

# Dynamic completion for powershell repeats last word when completing next argument

---

_Issue opened by @jakobhellermann on 2025-05-22 20:00_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.88.0-nightly (27d6200a7 2025-05-06)

### Clap Version

4.5.38

### Minimal reproducible code

```rust
use clap::{CommandFactory as _, Parser};

#[derive(Parser, Debug)]
#[command(version, name = "demo")]
struct Args {}

fn main() {
    clap_complete::env::CompleteEnv::with_factory(Args::command).complete();
}
```

### Steps to reproduce the bug with the above code


```
cargo install --path . --example demo
Invoke-Expression (& { (env COMPLETE=powershell C:/Users/user/.cargo/bin/demo | Out-String) })

demo --help <TAB>
```

### Actual Behaviour

```
demo --help --help
```

### Expected Behaviour

```
demo --help --
```

### Additional Context


This is because the powershell `EnvCompleter` determines the argument index based on the parsed args vec, which does not contain an empty element at the end:
https://github.com/clap-rs/clap/blob/e2188d9af318f3287c1c5a52cba6b9dfebe7bb75/clap_complete/src/env/shells.rs#L322

`Register-ArgumentCompleter` comes with `$cursorPosition`, but that gives you the actual column index in the command, which you can't trivially turn into an argument index.


## Possible Solutions
1. in the powershell glue code, select the correct word index based on `$commandAst.ToString()` and `$cursorPosition` and pass that via `_CLAP_COMPLETE_INDEX`
2. pass the raw `$commandAst.ToString()` and `$cursorPosition` to the binary and determine the argument index there
3. If we're at the end of the text, pass an extra empty argument to the binary: `& {completer} -- $commandAst ''`. That way it computes the argument index correctly.

Or maybe something I've missed.

### Debug Output

_No response_

---

_Label `C-bug` added by @jakobhellermann on 2025-05-22 20:00_

---

_Label `A-completion` added by @epage on 2025-05-22 20:29_

---

_Label `E-medium` added by @epage on 2025-05-22 20:29_

---

_Referenced in [clap-rs/clap#6166](../../clap-rs/clap/pulls/6166.md) on 2025-10-29 22:28_

---

_Closed by @epage on 2025-10-29 22:35_

---

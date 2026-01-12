```yaml
number: 6208
title: Optional value to argument not handled correctly in the generated zsh completion script
type: issue
state: open
author: saiarcot895
labels:
  - C-bug
  - S-triage
assignees: []
created_at: 2026-01-04T21:07:29Z
updated_at: 2026-01-04T21:07:29Z
url: https://github.com/clap-rs/clap/issues/6208
synced_at: 2026-01-12T16:14:17Z
```

# Optional value to argument not handled correctly in the generated zsh completion script

---

_@saiarcot895_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.89.0

### Clap Version

4.5.54

### Minimal reproducible code

```rust
use clap::{value_parser, Arg, Command};
use clap_complete::Shell;
use std::{io::stdout, path::PathBuf};

fn app() -> Command {
    Command::new("completion-bug")
        .arg(
            Arg::new("foo")
                .short('f')
                .long("foo")
                .value_parser(value_parser!(PathBuf))
                .num_args(0..=1),
        )
}

fn main() {
    clap_complete::generate(Shell::Zsh, &mut app(), "completion-bug", &mut stdout());
}
```


### Steps to reproduce the bug with the above code

1. `cargo build`
2. `target/debug/clap-test`
3. Observe that the generated completions have `'-f+[]'` and `'--foo=[]'`.

### Actual Behaviour

The generated completion doesn't allow for any value for that option, and if `--foo` is specified on the command line with a value, then zsh won't suggest any more options (i.e. if I type in `--foo=bar --` and hit tab, then there's no other completion options provided).

### Expected Behaviour

I would expect that the generated completion for zsh for `-f`/`--foo` would allow an optional value for the argument to be specified, and so would look something like `'--foo=[]:: :_files'` (based on the [zsh completion system documentation](https://zsh.sourceforge.io/Doc/Release/Completion-System.html) and some manual experiments).

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @saiarcot895 on 2026-01-04 21:07_

---

_Label `S-triage` added by @saiarcot895 on 2026-01-04 21:07_

---

_Referenced in [clap-rs/clap#6209](../../clap-rs/clap/pulls/6209.md) on 2026-01-04 21:07_

---

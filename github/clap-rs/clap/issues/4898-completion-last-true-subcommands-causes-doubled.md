---
number: 4898
title: "completion: `last(true)` + subcommands causes 'doubled rest argument' error in Zsh"
type: issue
state: closed
author: clubby789
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2023-05-10T17:29:42Z
updated_at: 2023-05-12T08:22:20Z
url: https://github.com/clap-rs/clap/issues/4898
synced_at: 2026-01-07T13:12:20-06:00
---

# completion: `last(true)` + subcommands causes 'doubled rest argument' error in Zsh

---

_Issue opened by @clubby789 on 2023-05-10 17:29_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

`rustc 1.71.0-nightly (f9a6b7158 2023-05-05)`

### Clap Version

4.2.7

### Minimal reproducible code

```rs
use clap::{CommandFactory, Parser, Subcommand};
use clap_complete::generate;

#[derive(Parser)]
struct Opt {
    #[arg(last(true))]
    pub free_args: Vec<String>,
    #[command(subcommand)]
    command: SubOpt,
}

#[derive(Subcommand)]
enum SubOpt {
    Foo,
    Bar,
}

fn main() {
    let mut cmd = Opt::command();
    let name = cmd.get_name().to_string();
    generate(clap_complete::shells::Zsh, &mut cmd, name, &mut std::io::stdout());
}
```

### Steps to reproduce the bug with the above code

Redirect the output to a file, use `source poc.zsh` to load it, then enter `<app-name>` and hit tab.

### Actual Behaviour

`_arguments:comparguments:327: doubled rest argument definition: *::: :->poc`

### Expected Behaviour

An error should not occur and completion should work as normal.

### Additional Context

I'm unsure if this is related to #4652, but it prevents completion from working *at all*

### Debug Output

_No response_

---

_Label `C-bug` added by @clubby789 on 2023-05-10 17:29_

---

_Renamed from "Global argument + `last(true)` + subcommands causes 'doubled rest argument' error in Zsh" to "completion: Global argument + `last(true)` + subcommands causes 'doubled rest argument' error in Zsh" by @clubby789 on 2023-05-10 17:29_

---

_Referenced in [rust-lang/rust#111388](../../rust-lang/rust/pulls/111388.md) on 2023-05-10 17:30_

---

_Renamed from "completion: Global argument + `last(true)` + subcommands causes 'doubled rest argument' error in Zsh" to "completion: `last(true)` + subcommands causes 'doubled rest argument' error in Zsh" by @clubby789 on 2023-05-10 17:55_

---

_Referenced in [clap-rs/clap#4899](../../clap-rs/clap/pulls/4899.md) on 2023-05-10 18:30_

---

_Label `A-completion` added by @epage on 2023-05-12 08:18_

---

_Label `E-easy` added by @epage on 2023-05-12 08:18_

---

_Closed by @epage on 2023-05-12 08:22_

---

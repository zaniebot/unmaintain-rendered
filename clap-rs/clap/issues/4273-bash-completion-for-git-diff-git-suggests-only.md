---
number: 4273
title: "Bash completion for `git diff git` suggests only top-level completions"
type: issue
state: closed
author: martinvonz
labels:
  - C-bug
  - E-hard
  - A-completion
assignees: []
created_at: 2022-09-28T18:49:40Z
updated_at: 2022-09-29T17:07:27Z
url: https://github.com/clap-rs/clap/issues/4273
synced_at: 2026-01-10T01:27:52Z
---

# Bash completion for `git diff git` suggests only top-level completions

---

_Issue opened by @martinvonz on 2022-09-28 18:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.63.0

### Clap Version

3.2.22

### Minimal reproducible code

```rust
use std::io::{stdout, Write};
use clap;
use clap::CommandFactory;

#[derive(clap::Parser, Clone, Debug)]
enum App {
    Diff,
}

fn main() {
    let mut app = App::command();
    let mut buf = vec![];
    clap_complete::generate(clap_complete::Shell::Bash, &mut app, "git", &mut buf);
    stdout().write_all(&mut buf).unwrap();
}
```


### Steps to reproduce the bug with the above code

Run the command and source the generated script. Then enter `git diff git <TAB>`

### Actual Behaviour

The suggestions include `diff`.

### Expected Behaviour

The suggestions should not include `diff`.

### Additional Context

This is a bug in the Bash completion script I noticed while working on #4265. The issue is that the initial processing loop in the generated Bash script resets the "command so far" variable (`$cmd`) to `git` whenever `git` appears in the list, so it provides suggestions for the top-level command.

### Debug Output

_No response_

---

_Label `C-bug` added by @martinvonz on 2022-09-28 18:49_

---

_Referenced in [clap-rs/clap#4280](../../clap-rs/clap/issues/4280.md) on 2022-09-28 20:40_

---

_Label `E-hard` added by @epage on 2022-09-28 21:29_

---

_Label `A-completion` added by @epage on 2022-09-28 21:29_

---

_Referenced in [clap-rs/clap#4284](../../clap-rs/clap/pulls/4284.md) on 2022-09-29 04:44_

---

_Referenced in [clap-rs/clap#4289](../../clap-rs/clap/pulls/4289.md) on 2022-09-29 16:44_

---

_Closed by @epage on 2022-09-29 17:07_

---

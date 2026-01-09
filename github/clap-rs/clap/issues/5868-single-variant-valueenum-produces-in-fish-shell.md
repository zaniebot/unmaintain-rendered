---
number: 5868
title: "Single Variant ValueEnum Produces \\{ in Fish Shell Completions"
type: issue
state: closed
author: tetzng
labels:
  - C-bug
  - E-medium
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2025-01-05T17:27:02Z
updated_at: 2025-01-09T16:32:51Z
url: https://github.com/clap-rs/clap/issues/5868
synced_at: 2026-01-07T13:12:20-06:00
---

# Single Variant ValueEnum Produces \{ in Fish Shell Completions

---

_Issue opened by @tetzng on 2025-01-05 17:27_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.83.0 (90b35a623 2024-11-26)

### Clap Version

4.5.23

### Minimal reproducible code

```rust
use clap::CommandFactory;

#[derive(clap::Parser)]
struct MyCommand {
    #[arg(long)]
    flag: Flag,
}

#[derive(Clone, clap::ValueEnum)]
enum Flag {
    /// Only single value
    Foo,
}

fn main() {
    clap_complete::generate(
        clap_complete::shells::Fish,
        &mut MyCommand::command(),
        "ls",
        &mut std::io::stdout(),
    );
```


### Steps to reproduce the bug with the above code

```fish
$ cargo run | source
$ ls --flag=<TAB>
```

### Actual Behaviour

```fish
$ ls --flag \{foo
```

### Expected Behaviour

```fish
$ ls --flag foo
```

### Additional Context

Zsh and Bash completions behave as expected.

### Debug Output

_No response_

---

_Label `C-bug` added by @tetzng on 2025-01-05 17:27_

---

_Label `A-completion` added by @epage on 2025-01-07 20:48_

---

_Label `E-medium` added by @epage on 2025-01-07 20:49_

---

_Label `E-help-wanted` added by @epage on 2025-01-07 20:49_

---

_Referenced in [clap-rs/clap#5874](../../clap-rs/clap/pulls/5874.md) on 2025-01-08 08:47_

---

_Closed by @epage on 2025-01-09 16:32_

---

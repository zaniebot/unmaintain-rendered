---
number: 5479
title: Add the Ability to Display Visible Aliases for PossibleValues
type: issue
state: closed
author: naftulikay
labels:
  - C-enhancement
assignees: []
created_at: 2024-05-01T22:23:13Z
updated_at: 2024-05-01T22:27:05Z
url: https://github.com/clap-rs/clap/issues/5479
synced_at: 2026-01-10T01:28:12Z
---

# Add the Ability to Display Visible Aliases for PossibleValues

---

_Issue opened by @naftulikay on 2024-05-01 22:23_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Consider the following use-case:

```rust
use clap::{Parser, ValueEnum};

#[derive(Debug, Clone, Parser)]
pub struct Args {
    /// The environment to execute in
    #[arg(short = 'e', long = "env")]
    pub env: Env,
}

#[derive(Debug, Clone, ValueEnum)]
pub enum Env {
    /// The development environment
    #[value(aliases = ["dev", "d"])]
    Development,
    /// The staging environment
    Staging,
    /// The production environment
    #[value(aliases = ["prod"])]
    Production,
}
```

In this case, `#[value(aliases = ["prod"])` adds an alias, but this alias is hidden, regardless of whether `hide` is set or not. The existing API allows these aliases but does not display them in `-h` or `--help` output.

### Describe the solution you'd like

To this end, I propose that we add a `visible_alias` and `visible_aliases` function to PossibleValue to allow users to add visible aliases which will show up in help text. PR is incoming.

### Alternatives, if applicable

This seems to be the most direct way of doing this.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @naftulikay on 2024-05-01 22:23_

---

_Referenced in [clap-rs/clap#5480](../../clap-rs/clap/pulls/5480.md) on 2024-05-01 22:23_

---

_Comment by @epage on 2024-05-01 22:27_

This seems to be the same as #4416, closing in favor of that.  If there is a reason to keep this open, let us know!

---

_Closed by @epage on 2024-05-01 22:27_

---

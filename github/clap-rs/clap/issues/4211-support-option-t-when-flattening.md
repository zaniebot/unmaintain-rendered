---
number: 4211
title: "Support `Option<T>` when flattening"
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - E-medium
  - E-help-wanted
  - A-derive
  - ":money_with_wings: $20"
  - S-blocked
assignees: []
created_at: 2022-09-13T14:49:10Z
updated_at: 2022-10-05T21:52:33Z
url: https://github.com/clap-rs/clap/issues/4211
synced_at: 2026-01-07T13:12:20-06:00
---

# Support `Option<T>` when flattening

---

_Issue opened by @epage on 2022-09-13 14:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.0

### Describe your use case

When flattening, I'd like to make the struct optional
```rust
#[derive(Clap)]
pub struct App{
    #[clap(flatten)]
    pub state_filter: Option<StateFilter>,
}

#[derive(Args)]
pub enum StateFilter {
    /// Only running servers are returned
    #[clap(long)]
    Running,
    /// Only exited servers are returned
    #[clap(long)]
    Exited,
    /// Only restarting servers are returned
    #[clap(long)]
    Restarting,
    #[clap(long)]
    Custom(String),
}
```

### Describe the solution you'd like

With #3165, we could check if the `ArgGroup` is present and, if it isn't, return `None`.  Otherwise, we walk into the `ArgGroup` and access it.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @epage on 2022-09-13 14:49_

---

_Label `E-medium` added by @epage on 2022-09-13 14:49_

---

_Label `A-derive` added by @epage on 2022-09-13 14:49_

---

_Label `:money_with_wings: $20` added by @epage on 2022-09-13 14:49_

---

_Label `S-blocked` added by @epage on 2022-09-13 14:49_

---

_Referenced in [clap-rs/clap#2621](../../clap-rs/clap/issues/2621.md) on 2022-09-13 14:49_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:34_

---

_Referenced in [clap-rs/clap#4279](../../clap-rs/clap/issues/4279.md) on 2022-09-28 22:16_

---

_Added to milestone `4.x` by @epage on 2022-09-29 20:31_

---

_Referenced in [clap-rs/clap#4350](../../clap-rs/clap/pulls/4350.md) on 2022-10-05 21:27_

---

_Comment by @epage on 2022-10-05 21:30_

Oops, looks like we already have #3123, closing in favor of that though #4350 is just about to be merged, supporting this

---

_Closed by @epage on 2022-10-05 21:30_

---

_Comment by @epage on 2022-10-05 21:52_

v4.0.10

---

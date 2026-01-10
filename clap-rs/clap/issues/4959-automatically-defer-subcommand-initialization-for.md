---
number: 4959
title: Automatically defer subcommand initialization for derives
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - M-breaking-change
  - A-derive
assignees: []
created_at: 2023-06-09T14:32:56Z
updated_at: 2023-06-09T15:07:37Z
url: https://github.com/clap-rs/clap/issues/4959
synced_at: 2026-01-10T01:28:04Z
---

# Automatically defer subcommand initialization for derives

---

_Issue opened by @epage on 2023-06-09 14:32_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.3.3

### Describe your use case

#4792 added deferred command initialization but no support exists yet for the derive API.

### Describe the solution you'd like

Automatically defer all non-essential subcommand calls.

### Alternatives, if applicable

_No response_

### Additional Context

Automatic deferment would be a breaking change though we could make it opt-in through the remainder of 4.x

A rough sketch
- Extend `Args` to have `defer_augment_args`
- Extend `Subcommand` to have `defer_augment_subcommand`
- New `#[command(defer = <bool>)]` magic attribute
  - defaults to `false` in clap v4
  - defaults to `true` in clap v5, mark it as deprecated
  - remove it in clap v6
- If `defer = false` on a top-level container, always call `defer_augment_args` along with `augment_args`
- If `defer = true` on a top-level container
  - Defer `Command::subcommand` and `Command::arg` calls
  - if `defer = true` on a `flatten`, `augment_args` only calls the same of its children
  - If `defer = false` on a `flatten`, defer `augment_args` until when `defer_augment_args` is called

---

_Label `C-enhancement` added by @epage on 2023-06-09 14:32_

---

_Label `M-breaking-change` added by @epage on 2023-06-09 14:32_

---

_Label `A-derive` added by @epage on 2023-06-09 14:32_

---

_Added to milestone `5.0` by @epage on 2023-06-09 14:32_

---

_Referenced in [clap-rs/clap#4792](../../clap-rs/clap/pulls/4792.md) on 2023-06-09 14:33_

---

_Referenced in [clap-rs/clap#4956](../../clap-rs/clap/issues/4956.md) on 2023-06-09 15:13_

---

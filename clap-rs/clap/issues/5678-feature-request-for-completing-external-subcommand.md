---
number: 5678
title: Feature request for completing external subcommand
type: issue
state: closed
author: shannmu
labels:
  - C-enhancement
assignees: []
created_at: 2024-08-16T08:09:54Z
updated_at: 2024-08-16T10:00:02Z
url: https://github.com/clap-rs/clap/issues/5678
synced_at: 2026-01-10T01:28:15Z
---

# Feature request for completing external subcommand

---

_Issue opened by @shannmu on 2024-08-16 08:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Cargo can invoke external commands. If there is a `cargo-foo` binary in the `PATH` environment variable, we can run it like a Cargo subcommand by using `cargo foo`.
What i want is to complete `cargo fo[TAB]` to `cargo foo`

### Describe the solution you'd like

- Add a struct like `SubcommadCompleter`
- impl `CustomCompleter` for `SubcommadCompleter`
- invoke the complete callback when there is no positional argument on Command, or the pos_index is 1.


### Additional Context
See https://github.com/clap-rs/clap/issues/3166 to get more context

---

_Label `C-enhancement` added by @shannmu on 2024-08-16 08:09_

---

_Closed by @shannmu on 2024-08-16 10:00_

---

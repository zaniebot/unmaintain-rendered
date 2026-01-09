---
number: 3553
title: "clap_generate: completion on subcommand is always default, even if it takes no values"
type: issue
state: closed
author: Icelk
labels:
  - C-enhancement
  - A-completion
  - E-easy
assignees: []
created_at: 2022-03-12T22:46:30Z
updated_at: 2024-08-10T00:41:43Z
url: https://github.com/clap-rs/clap/issues/3553
synced_at: 2026-01-07T13:12:19-06:00
---

# clap_generate: completion on subcommand is always default, even if it takes no values

---

_Issue opened by @Icelk on 2022-03-12 22:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.0

### Describe your use case

When generating completions for my application, Fish suggests file names after inputting a subcommand: `std-dev regression<tab>` gives me file name suggestions. This subcommand doesn't have any args without flags (nothing under the `ARGS:` section of the help output).

### Describe the solution you'd like

I suggest telling Fish (and other shells?) to not complete anything, or, optimally, begin completing flags & options.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Icelk on 2022-03-12 22:46_

---

_Label `A-completion` added by @epage on 2022-03-14 15:22_

---

_Label `E-easy` added by @epage on 2022-03-14 15:22_

---

_Comment by @epage on 2024-08-10 00:41_

I believe this is resolved in our rust-native completions.  If not, let us know and we can re-open.

Stabilization can be tracked in #3166

---

_Closed by @epage on 2024-08-10 00:41_

---

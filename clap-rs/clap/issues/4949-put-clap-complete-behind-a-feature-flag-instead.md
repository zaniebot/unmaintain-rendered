---
number: 4949
title: put clap_complete behind a feature flag instead of a separate crate
type: issue
state: closed
author: contagnas
labels:
  - C-enhancement
  - A-completion
  - S-wont-fix
assignees: []
created_at: 2023-06-05T00:45:15Z
updated_at: 2023-06-05T12:08:52Z
url: https://github.com/clap-rs/clap/issues/4949
synced_at: 2026-01-10T01:28:04Z
---

# put clap_complete behind a feature flag instead of a separate crate

---

_Issue opened by @contagnas on 2023-06-05 00:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Currently clap_complete is shipped as a separate crate. This hurts discoverability (at least for me), and means users need to keep the dependency versions in sync.

### Describe the solution you'd like

Put clap_complete behind a feature flag, like derive

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @contagnas on 2023-06-05 00:45_

---

_Referenced in [clap-rs/clap#4950](../../clap-rs/clap/pulls/4950.md) on 2023-06-05 00:48_

---

_Comment by @epage on 2023-06-05 12:08_

clap used to include completions (see [`App::gen_completions`](https://docs.rs/clap/2.32.0/clap/struct.App.html#method.gen_completions) and it was intentionally split out as part of the v3 effort.

Features have several usability problems in Rust though some are made up for with some unstable rustdoc features we use with docs.rs.

In my mind, the better route for us to be going is to leverage https://github.com/rust-lang/rfcs/pull/3243# when its approved.

---

_Closed by @epage on 2023-06-05 12:08_

---

_Label `A-completion` added by @epage on 2023-06-05 12:08_

---

_Label `S-wont-fix` added by @epage on 2023-06-05 12:08_

---

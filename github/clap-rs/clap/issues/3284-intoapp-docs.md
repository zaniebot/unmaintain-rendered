---
number: 3284
title: "`IntoApp` docs"
type: issue
state: closed
author: nipunn1313
labels:
  - C-bug
assignees: []
created_at: 2022-01-11T23:36:45Z
updated_at: 2022-01-12T16:59:28Z
url: https://github.com/clap-rs/clap/issues/3284
synced_at: 2026-01-07T13:12:19-06:00
---

# `IntoApp` docs

---

_Issue opened by @nipunn1313 on 2022-01-11 23:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.0-nightly (b426445c6 2021-11-24)

### Clap Version

3.0.6

### Minimal reproducible code

Docs in https://docs.rs/clap/latest/clap/trait.IntoApp.html indicate that it's derived on Parser, Args, and Subcommand, but afaict it's only derived on Parser. Came across it while trying to figure out if it was possible to do `MySubcommand::into_app().debug_assert()`

### Steps to reproduce the bug with the above code

It's a doc bug

### Actual Behaviour

see above

### Expected Behaviour

Docs should be consistent.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @nipunn1313 on 2022-01-11 23:36_

---

_Closed by @epage on 2022-01-12 16:59_

---

_Comment by @epage on 2022-01-12 16:59_

Thanks for calling this out!

---

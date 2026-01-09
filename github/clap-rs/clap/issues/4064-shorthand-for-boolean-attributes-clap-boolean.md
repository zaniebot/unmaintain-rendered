---
number: 4064
title: "Shorthand for boolean attributes: `#[clap(boolean_attribute = true)]` should be shortened to `#[clap(boolean_attribute)]` (unless it was `true` by default)"
type: issue
state: closed
author: KSXGitHub
labels:
  - C-enhancement
  - S-duplicate
assignees: []
created_at: 2022-08-11T09:25:41Z
updated_at: 2022-08-11T16:59:11Z
url: https://github.com/clap-rs/clap/issues/4064
synced_at: 2026-01-07T13:12:20-06:00
---

# Shorthand for boolean attributes: `#[clap(boolean_attribute = true)]` should be shortened to `#[clap(boolean_attribute)]` (unless it was `true` by default)

---

_Issue opened by @KSXGitHub on 2022-08-11 09:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.16

### Describe your use case

I had found myself needing to set boolean attributes to `true` (i.e. passing `true` to a boolean method of `builder::Args`) multiple times, making the code looks somewhat ugly.

```rust
#[derive(Debug, Parser)]
struct MyProgram {
    #[clap(long = "exec", short = 'x', multiple_values = true, raw = true)]
    command: Vec<OsString>,
}
```

### Describe the solution you'd like

The `= true` part should be optional:

```rust
#[derive(Debug, Parser)]
struct MyProgram {
    #[clap(long = "exec", short = 'x', multiple_values, raw)]
    command: Vec<OsString>,
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @KSXGitHub on 2022-08-11 09:25_

---

_Comment by @epage on 2022-08-11 16:59_

We are currently tracking this in #3620

---

_Closed by @epage on 2022-08-11 16:59_

---

_Label `S-duplicate` added by @epage on 2022-08-11 16:59_

---

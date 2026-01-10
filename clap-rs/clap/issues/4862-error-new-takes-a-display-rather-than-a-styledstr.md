---
number: 4862
title: "`Error::new` takes a `Display` rather than a `StyledStr`"
type: issue
state: open
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2023-04-26T16:49:46Z
updated_at: 2023-04-26T16:50:29Z
url: https://github.com/clap-rs/clap/issues/4862
synced_at: 2026-01-10T01:28:02Z
---

# `Error::new` takes a `Display` rather than a `StyledStr`

---

_Issue opened by @epage on 2023-04-26 16:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.69

### Clap Version

4.2.4

### Minimal reproducible code

N/A

### Steps to reproduce the bug with the above code

N/A

### Actual Behaviour

The API is specified as not allowing user formatted text

### Expected Behaviour

The API is specified to accept user formatted text

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2023-04-26 16:49_

---

_Label `M-breaking-change` added by @epage on 2023-04-26 16:49_

---

_Label `A-help` added by @epage on 2023-04-26 16:49_

---

_Label `S-waiting-on-design` added by @epage on 2023-04-26 16:49_

---

_Added to milestone `5.0` by @epage on 2023-04-26 16:49_

---

_Comment by @epage on 2023-04-26 16:50_

As a workaround, it looks like it should work to pass in styled text even if the API doesn't specify it accepts it

---

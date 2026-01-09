---
number: 5087
title: The latest update enforces 1.7.9 rust version, breaking build pipelines.
type: issue
state: closed
author: rikkigouda
labels:
  - C-bug
assignees: []
created_at: 2023-08-24T23:08:17Z
updated_at: 2023-08-24T23:40:28Z
url: https://github.com/clap-rs/clap/issues/5087
synced_at: 2026-01-07T13:12:20-06:00
---

# The latest update enforces 1.7.9 rust version, breaking build pipelines.

---

_Issue opened by @rikkigouda on 2023-08-24 23:08_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.69 (Dockerfile)

### Clap Version

latest

### Minimal reproducible code

Our pipelines have started breaking down due to the following error. After investigating, we found that clap crate has been updated enforcing newer rust version we do not use.

```
 #12 147.0 error: package `clap v4.4.0` cannot be built because it requires rustc 1.70.0 or newer, while the currently active rustc version is 1.69.0
```


### Steps to reproduce the bug with the above code

Run `cargo build` against an existing rust project using `1.69` rust version.

### Actual Behaviour

The build pipelines fail despite the fact that we have introduced no change.

### Expected Behaviour

Build must stay green for projects relying on older rust versions (compatiblity).

### Additional Context

We do not have clap in our toml file.

### Debug Output

_No response_

---

_Label `C-bug` added by @rikkigouda on 2023-08-24 23:08_

---

_Comment by @epage on 2023-08-24 23:40_

Our MSRV policy is N-2 which this is in accordance with. In #3267, it was suggested to extend it but that was rejected.

I'm assuming you are not committing a lockfile. For projects that care about MSRV, I recommend you do. If you are concerned with cargo's policy, the cargo nightly documentation removes the discouragement to ignore lockfiles. I'm on mobile right now or else id provide the link.

---

_Closed by @epage on 2023-08-24 23:40_

---

_Referenced in [esp-rs/espflash#467](../../esp-rs/espflash/pulls/467.md) on 2023-08-27 03:34_

---

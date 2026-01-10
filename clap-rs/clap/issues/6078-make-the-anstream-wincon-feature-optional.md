---
number: 6078
title: "Make the `anstream/wincon` feature optional"
type: issue
state: closed
author: dullbananas
labels:
  - C-enhancement
assignees: []
created_at: 2025-07-28T22:18:33Z
updated_at: 2025-08-03T00:15:29Z
url: https://github.com/clap-rs/clap/issues/6078
synced_at: 2026-01-10T01:28:21Z
---

# Make the `anstream/wincon` feature optional

---

_Issue opened by @dullbananas on 2025-07-28 22:18_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

clap_builder 4.5.40

### Describe your use case

reducing dependencies

### Describe the solution you'd like

disable default features of anstream, and add a feature to clap that enables optional features (at least wincon)

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @dullbananas on 2025-07-28 22:18_

---

_Comment by @epage on 2025-07-28 22:53_

This is asking for a specific outcome.  Could you describe the real-world problem you are facing with this dependency?

Also for easy reference, here is the `cargo tree` output with default features

<img width="705" height="570" alt="Image" src="https://github.com/user-attachments/assets/7c3e6b67-4857-4113-91dc-cf5c246e2360" />

---

_Comment by @dullbananas on 2025-07-29 22:39_

What I want is faster compilation, and less code to inspect if someone wants to inspect the code of all dependencies of a project.

---

_Comment by @epage on 2025-07-29 22:44_

Can you provide a timings report to show the impact the dependency is having, rather than being theoretical?

---

_Comment by @dullbananas on 2025-08-03 00:15_

I just found out that anstream uses `#[cfg(all(windows, feature = "wincon"))]` and `[target.'cfg(windows)'.dependencies]`, so this is not a problem. I falsely thought that the presence of Windows stuff in Cargo.lock means that they are being compiled.

---

_Closed by @dullbananas on 2025-08-03 00:15_

---

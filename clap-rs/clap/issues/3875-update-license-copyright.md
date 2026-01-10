---
number: 3875
title: Update license copyright
type: issue
state: closed
author: Sytten
labels:
  - C-enhancement
assignees: []
created_at: 2022-06-27T17:17:19Z
updated_at: 2022-06-27T20:35:37Z
url: https://github.com/clap-rs/clap/issues/3875
synced_at: 2026-01-10T01:27:48Z
---

# Update license copyright

---

_Issue opened by @Sytten on 2022-06-27 17:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.5

### Describe your use case

The copyright on the MIT license is widely out of date.
This makes is harder to give proper attribution for the project.
The author field is also missing from the Cargo.toml

### Describe the solution you'd like

At least update the copyright of clap to something like `Clap Core Team`.
If possible also add this to the `author` of the `Cargo.toml`
Thanks!

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Sytten on 2022-06-27 17:17_

---

_Comment by @epage on 2022-06-27 17:57_

I've updated  the years and noted that the copyright is owned by the contributors.

I'm leaving out author field as I've not been a fan of it (leaving people out, generic "team" authors being useless) and was glad when cargo switched to it being optional (the motivation for that was that names are not immutable)

---

_Comment by @Sytten on 2022-06-27 20:35_

Make sense, it's just a pain when you try to automate license attribution but all good. Thanks!

---

_Closed by @Sytten on 2022-06-27 20:35_

---

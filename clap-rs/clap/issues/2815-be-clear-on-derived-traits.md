---
number: 2815
title: Be clear on derived traits
type: issue
state: closed
author: epage
labels:
  - A-docs
assignees: []
created_at: 2021-10-05T15:22:20Z
updated_at: 2021-10-12T16:28:21Z
url: https://github.com/clap-rs/clap/issues/2815
synced_at: 2026-01-10T01:27:27Z
---

# Be clear on derived traits

---

_Issue opened by @epage on 2021-10-05 15:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

v3.0.0-beta.4

### Where?

Everywhere

### What's wrong?

For items we `flatten` and `subcommand`, we are deriving `Clap` when we should consider deriving `Args` or `Subcommand`.

### How to fix?

Scrub the docs, updating them

---

_Label `C: docs` added by @epage on 2021-10-05 15:22_

---

_Added to milestone `3.0` by @epage on 2021-10-05 15:22_

---

_Referenced in [clap-rs/clap#2814](../../clap-rs/clap/pulls/2814.md) on 2021-10-05 15:25_

---

_Closed by @epage on 2021-10-12 16:05_

---

_Comment by @pksunkara on 2021-10-12 16:15_

Can you please add what the reason for closure is? I am not sure if this issue is about docs for clap derive or it is for scrubbing the examples and tests about which derive macro to use.

---

_Comment by @epage on 2021-10-12 16:28_

The main docs we have atm are in `src/derive.rs` which already make the distinction to a degree.  The remaining work was what was done in #2814.  As we work on #2856, we'll also have to keep this in mind but that is a part of that issue and not this.

---

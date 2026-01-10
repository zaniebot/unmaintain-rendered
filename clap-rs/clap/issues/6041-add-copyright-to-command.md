---
number: 6041
title: Add copyright to Command
type: issue
state: closed
author: dcampbell24
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2025-06-22T19:19:28Z
updated_at: 2025-06-24T00:47:39Z
url: https://github.com/clap-rs/clap/issues/6041
synced_at: 2026-01-10T01:28:21Z
---

# Add copyright to Command

---

_Issue opened by @dcampbell24 on 2025-06-22 19:19_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Optionally, change the display of `--version` and add a `COPYRIGHT` section to `clap_mangen` generated man pages.

### Describe the solution you'd like

I want to add a `copyright` function to `Command` that causes version to also display the copyright and add a `COPYRIGHT` section to `clap_mangen` generated man pages.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @dcampbell24 on 2025-06-22 19:19_

---

_Referenced in [clap-rs/clap#6043](../../clap-rs/clap/pulls/6043.md) on 2025-06-22 21:07_

---

_Comment by @epage on 2025-06-23 13:44_

What value does a distinct `copyright` function have over providing a custom formatted `version` / `long_version`?

From clap' perspective, I would also be interested in answers to the questions from rust-lang/cargo#15697

---

_Label `A-help` added by @epage on 2025-06-23 13:44_

---

_Label `S-waiting-on-design` added by @epage on 2025-06-23 13:44_

---

_Comment by @dcampbell24 on 2025-06-23 17:33_

How do I add a `COPYRIGHT` section? I don't want it to be called `VERSION`?

---

_Comment by @epage on 2025-06-23 18:05_

This Issue is about a `Command::copyright` affecting both `--version` and `clap_mangen`.

For `--version`, you can provide a custom version, tailored to how you want it formatted.

For `clap_mangen`, as I mentioned for licenses at https://github.com/clap-rs/clap/issues/1768#issuecomment-2996516939, we don't expect a well crafted man page to be able to be fully specified via `clap` and that people will need to add their own custom sections (config and environment variables are others that would be relevant that clap should not cover).

---

_Comment by @dcampbell24 on 2025-06-23 20:30_

If I customize  `--version` it works alright on the command line, but then `clap_mangen` includes my additions under `VERSION` in the man page and I already included the `COPYRIGHT` details by extending the man page by hand.

---

_Comment by @dcampbell24 on 2025-06-24 00:47_

I disabled the display of version for the man page. It is a non-standard section anyway. It is a little odd, but I  guess everything works as is.

---

_Closed by @dcampbell24 on 2025-06-24 00:47_

---

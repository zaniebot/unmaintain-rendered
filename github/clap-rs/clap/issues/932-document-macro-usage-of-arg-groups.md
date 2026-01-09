---
number: 932
title: Document macro usage of arg groups
type: issue
state: closed
author: TedDriggs
labels:
  - C-enhancement
  - A-docs
  - E-medium
assignees: []
created_at: 2017-04-12T22:34:40Z
updated_at: 2018-08-02T03:30:05Z
url: https://github.com/clap-rs/clap/issues/932
synced_at: 2026-01-07T13:12:19-06:00
---

# Document macro usage of arg groups

---

_Issue opened by @TedDriggs on 2017-04-12 22:34_

### Rust Version

rustc 1.18.0-nightly (3b5754e5c 2017-04-10)

### Affected Version of clap

2.23

### Missing Documentation

The macro syntax for declaring applications doesn't make clear how to use arg groups. I attempted to follow the rules for args, such as using `+required`, but this failed saying an ident was expected. I've tried a variety of different formats without success, so this is a general documentation or example request rather than an ask for specific debugging help.

---

_Comment by @kbknapp on 2017-04-18 00:50_

Good point! Thanks. This is because those [shorthand tricks only work with args](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/macros.rs#L705).

Although I guess there's really no reason why those couldn't be [added for groups here](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/macros.rs#L650-L659).

---

_Label `C: arg groups` added by @kbknapp on 2017-04-18 00:51_

---

_Label `C: docs` added by @kbknapp on 2017-04-18 00:51_

---

_Label `C: macros` added by @kbknapp on 2017-04-18 00:51_

---

_Label `D: easy` added by @kbknapp on 2017-04-18 00:51_

---

_Label `M: mentored` added by @kbknapp on 2017-04-18 00:51_

---

_Label `P3: want to have` added by @kbknapp on 2017-04-18 00:51_

---

_Label `T: enhancement` added by @kbknapp on 2017-04-18 00:51_

---

_Label `W: 2.x` added by @kbknapp on 2017-04-18 00:51_

---

_Added to milestone `2.24.2` by @kbknapp on 2017-05-09 19:01_

---

_Closed by @kbknapp on 2017-05-16 11:23_

---

---
number: 625
title: Provide method to NOT wrap at any length
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2016-08-23T02:46:42Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/625
synced_at: 2026-01-07T13:12:19-06:00
---

# Provide method to NOT wrap at any length

---

_Issue opened by @kbknapp on 2016-08-23 02:46_

Currently, clap wraps long help text lines at 120 chars if no term width can be determined (i.e. Windows). There should be a way for one to say, "Don't wrap at all" such as saying `App::set_term_width(0)` or something similar

cc https://github.com/rust-lang-nursery/rustup.rs/issues/657


---

_Label `T: enhancement` added by @kbknapp on 2016-08-23 02:47_

---

_Label `P2: need to have` added by @kbknapp on 2016-08-23 02:47_

---

_Label `C: help message` added by @kbknapp on 2016-08-23 02:47_

---

_Label `D: easy` added by @kbknapp on 2016-08-23 02:47_

---

_Label `W: 2.x` added by @kbknapp on 2016-08-23 02:47_

---

_Comment by @kbknapp on 2016-08-25 01:55_

This is added with #628 


---

_Closed by @homu on 2016-08-25 02:42_

---

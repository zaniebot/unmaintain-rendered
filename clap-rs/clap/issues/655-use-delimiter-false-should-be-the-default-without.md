---
number: 655
title: use_delimiter(false) should be the default without multiples
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
assignees: []
created_at: 2016-09-10T19:18:19Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/655
synced_at: 2026-01-10T01:26:33Z
---

# use_delimiter(false) should be the default without multiples

---

_Issue opened by @kbknapp on 2016-09-10 19:18_

This has been an issue with a few projects. Because `use_delimiter(true)` is the default, which is meant for multiple values, but it takes affect even with a single value for an option or arg. This means a valid single value, `--option "some thing, with commas, and stuff"` will get split up and causes confusing errors.

`use_delimiter(false)` should be default _unless_ `multiple(true)` or similar has been set (such as `number_of_values` etc.)


---

_Label `T: enhancement` added by @kbknapp on 2016-09-10 19:18_

---

_Label `P2: need to have` added by @kbknapp on 2016-09-10 19:18_

---

_Label `C: args` added by @kbknapp on 2016-09-10 19:18_

---

_Label `D: easy` added by @kbknapp on 2016-09-10 19:18_

---

_Label `W: 2.x` added by @kbknapp on 2016-09-10 19:18_

---

_Added to milestone `2.12.0` by @kbknapp on 2016-09-10 19:18_

---

_Referenced in [DanielKeep/cargo-script#28](../../DanielKeep/cargo-script/issues/28.md) on 2016-09-10 19:49_

---

_Closed by @homu on 2016-09-11 02:39_

---

---
number: 960
title: "`--` takes (incorrectly) takes precedence over allowing hyphen values"
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-05-15T17:39:51Z
updated_at: 2018-08-02T03:30:07Z
url: https://github.com/clap-rs/clap/issues/960
synced_at: 2026-01-10T01:26:39Z
---

# `--` takes (incorrectly) takes precedence over allowing hyphen values

---

_Issue opened by @kbknapp on 2017-05-15 17:39_

See BurntSushi/ripgrep#482

The error stems from [`src/app/parser.rs#803`](https://github.com/kbknapp/clap-rs/blob/34a7436dd366da1a046e00310e96357ccf2dd533/src/app/parser.rs#L803) which isn't considering if we're currently parsing a value or not and whether that value allows leading hyphens.

My only concern is args like ripgrep's `-e` which allow multiple values, is there a case where `-e -- -- arg1` would be valid? Where the second `--` is meant as the *true* "only positional args follow". (NB ripgrep specifically actually avoids this edge case by [correctly] only allowing one value per `-e` invocation.)

---

_Label `C: parsing` added by @kbknapp on 2017-05-15 17:40_

---

_Label `D: easy` added by @kbknapp on 2017-05-15 17:40_

---

_Label `P2: need to have` added by @kbknapp on 2017-05-15 17:40_

---

_Label `T: bug` added by @kbknapp on 2017-05-15 17:40_

---

_Label `W: 2.x` added by @kbknapp on 2017-05-15 17:40_

---

_Added to milestone `2.24.2` by @kbknapp on 2017-05-15 17:40_

---

_Closed by @kbknapp on 2017-05-16 11:23_

---

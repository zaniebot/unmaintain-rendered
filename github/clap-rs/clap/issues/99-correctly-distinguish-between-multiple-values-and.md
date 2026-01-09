---
number: 99
title: Correctly distinguish between multiple values, and multiple allowances
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
assignees: []
created_at: 2015-05-04T04:05:24Z
updated_at: 2015-05-05T02:04:04Z
url: https://github.com/clap-rs/clap/issues/99
synced_at: 2026-01-07T13:12:19-06:00
---

# Correctly distinguish between multiple values, and multiple allowances

---

_Issue opened by @kbknapp on 2015-05-04 04:05_

This is a _slightly_ breaking change - not enough to warrant a label.

Currently, using named values `-f <file> <mode>` requires you to set `multiple(true)`...but this isn't correct logic. By this token, someone could legally do:

``` sh
$ myprog -f some.txt fast -f other.txt slow
```

This may, or _may not_, be what the developer wanted. Setting `multiple(true)` shouldn't be a requirement for named values...only for **unnamed** multiple values. This allows the developer to decide which style of argument he wants, multiple allowances, or multiple values.


---

_Label `enhancement` added by @kbknapp on 2015-05-04 04:05_

---

_Added to milestone `1.0 Release` by @kbknapp on 2015-05-04 04:07_

---

_Assigned to @kbknapp by @kbknapp on 2015-05-04 04:19_

---

_Closed by @kbknapp on 2015-05-05 02:04_

---

_Referenced in [clap-rs/clap#1020](../../clap-rs/clap/issues/1020.md) on 2017-08-08 09:39_

---

---
number: 444
title: Add support for deriving display order from declaration order
type: issue
state: closed
author: kbknapp
labels:
  - A-help
assignees: []
created_at: 2016-03-10T14:01:57Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/444
synced_at: 2026-01-07T13:12:19-06:00
---

# Add support for deriving display order from declaration order

---

_Issue opened by @kbknapp on 2016-03-10 14:01_

Derive the display order from the declaration order.

I.e. the display order of arguments and subcommnds in the help text would be the same order as declared.

Relates to #442 and something like is discussed in [multirust-rs](https://github.com/Diggsey/multirust-rs/issues/88)


---

_Label `C: args` added by @kbknapp on 2016-03-10 14:01_

---

_Label `C: help message` added by @kbknapp on 2016-03-10 14:01_

---

_Label `C: subcommands` added by @kbknapp on 2016-03-10 14:01_

---

_Label `D: easy` added by @kbknapp on 2016-03-10 14:01_

---

_Label `P3: want to have` added by @kbknapp on 2016-03-10 14:01_

---

_Label `T: new setting` added by @kbknapp on 2016-03-10 14:01_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-10 14:01_

---

_Comment by @kbknapp on 2016-03-10 18:12_

This will be opt-in via a `AppSettings::DeriveDisplayOrder`


---

_Comment by @kbknapp on 2016-03-10 21:38_

Implemented via #446 


---

_Closed by @homu on 2016-03-10 23:25_

---

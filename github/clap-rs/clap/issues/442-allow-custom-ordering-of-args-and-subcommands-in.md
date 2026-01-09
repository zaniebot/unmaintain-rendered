---
number: 442
title: Allow custom ordering of args and subcommands in help message
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2016-03-09T12:38:26Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/442
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow custom ordering of args and subcommands in help message

---

_Issue opened by @kbknapp on 2016-03-09 12:38_

I'm imagining adding something like a `display_order` (or whatever it gets called) method which sets the display order. Items with the same order will be listed alphabetically and the default will be something like 999 so the custom orders appear first and have a low probability for being accidentally mixed in with defaults. This would also allow some overlap so that maintenance should be low.

This should help when there are arguments or subcommands you'd like to highlight or that are most frequently used and you don't want to overwrite the entire help message by hand.

Such as something like is discussed in [multirust-rs](https://github.com/Diggsey/multirust-rs/issues/88)


---

_Label `T: enhancement` added by @kbknapp on 2016-03-09 12:38_

---

_Label `C: args` added by @kbknapp on 2016-03-09 12:38_

---

_Label `C: help message` added by @kbknapp on 2016-03-09 12:38_

---

_Label `C: subcommands` added by @kbknapp on 2016-03-09 12:38_

---

_Label `D: intermediate` added by @kbknapp on 2016-03-09 12:38_

---

_Label `P3: want to have` added by @kbknapp on 2016-03-09 12:38_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-09 12:38_

---

_Comment by @kbknapp on 2016-03-09 21:44_

Implemented by #443 


---

_Closed by @homu on 2016-03-10 02:42_

---

_Referenced in [clap-rs/clap#444](../../clap-rs/clap/issues/444.md) on 2016-03-10 14:02_

---

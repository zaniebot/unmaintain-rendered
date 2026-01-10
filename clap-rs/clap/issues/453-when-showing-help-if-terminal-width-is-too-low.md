---
number: 453
title: When showing help, If terminal width is too low, clap goes into an infinite loop
type: issue
state: closed
author: crumblingstatue
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-03-16T08:18:23Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/453
synced_at: 2026-01-10T01:26:29Z
---

# When showing help, If terminal width is too low, clap goes into an infinite loop

---

_Issue opened by @crumblingstatue on 2016-03-16 08:18_

If 01a_quick_example is invoked with the help subcommand on a terminal of width 34 or lower, clap goes into an infinite loop. It also keeps allocating memory.


---

_Referenced in [clap-rs/clap#450](../../clap-rs/clap/pulls/450.md) on 2016-03-16 08:18_

---

_Comment by @crumblingstatue on 2016-03-16 09:01_

[this](https://github.com/kbknapp/clap-rs/blob/cc127f982515dc5785469347509b1628441dd2ae/src/args/help_writer.rs#L162-L175) is the problematic loop.


---

_Label `T: bug` added by @kbknapp on 2016-03-16 13:07_

---

_Label `P1: urgent` added by @kbknapp on 2016-03-16 13:07_

---

_Label `C: args` added by @kbknapp on 2016-03-16 13:07_

---

_Label `C: help message` added by @kbknapp on 2016-03-16 13:07_

---

_Label `D: easy` added by @kbknapp on 2016-03-16 13:07_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-16 13:07_

---

_Comment by @kbknapp on 2016-03-16 13:08_

Since this is a bigger issue this'll take priority but should also be an easy fix. Thanks again!


---

_Comment by @kbknapp on 2016-03-16 19:31_

I have this fixed locally. Once I get home I'll put in the PR and put out 2.2.1 after the merge


---

_Closed by @homu on 2016-03-17 13:32_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-08-22 04:18_

---

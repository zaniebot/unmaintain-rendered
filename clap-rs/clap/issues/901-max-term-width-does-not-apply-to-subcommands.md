---
number: 901
title: max_term_width does not apply to subcommands
type: issue
state: closed
author: mitsuhiko
labels:
  - C-bug
  - A-help
  - E-medium
assignees: []
created_at: 2017-03-14T16:13:17Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/901
synced_at: 2026-01-10T01:26:38Z
---

# max_term_width does not apply to subcommands

---

_Issue opened by @mitsuhiko on 2017-03-14 16:13_

Currently if one sets `max_term_width` it does not apply to subcommands it seems which means one has to duplicate this for every subcommand. Would be great if this could be set globally.

---

_Comment by @kbknapp on 2017-03-14 18:43_

I agree, and think this may be a regression bug. Thanks for filling!

---

_Label `C: help message` added by @kbknapp on 2017-03-14 18:44_

---

_Label `D: easy` added by @kbknapp on 2017-03-14 18:44_

---

_Label `P2: need to have` added by @kbknapp on 2017-03-14 18:44_

---

_Label `Regression` added by @kbknapp on 2017-03-14 18:44_

---

_Label `T: bug` added by @kbknapp on 2017-03-14 18:44_

---

_Label `W: 2.x` added by @kbknapp on 2017-03-14 18:44_

---

_Comment by @kbknapp on 2017-03-14 18:45_

I'm on mobile, but if someone would like to put in a PR, this should be handled in `Parser:: propagate_settings` and should only require a line or two.

---

_Label `M: mentored` added by @kbknapp on 2017-03-14 18:46_

---

_Referenced in [clap-rs/clap#904](../../clap-rs/clap/pulls/904.md) on 2017-03-16 13:21_

---

_Comment by @mitsuhiko on 2017-03-16 13:23_

I pushed a PR up that you can merge. Note though that the function actually has a typo. I did not rename it because it's public but it might make sense to eventually rename it.

---

_Comment by @kbknapp on 2017-03-16 13:31_

:+1: it could actually be renamed because "crate internal public" but I can also rename it later too.

---

_Closed by @homu on 2017-03-16 14:16_

---

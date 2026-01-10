---
number: 515
title: Extra Newlines
type: issue
state: closed
author: mitsuhiko
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-05-28T19:31:47Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/515
synced_at: 2026-01-10T01:26:30Z
---

# Extra Newlines

---

_Issue opened by @mitsuhiko on 2016-05-28 19:31_

Currently clap renders an extra newline after usage, version and help. This is untypical and there does not appear to be a way to disable that.


---

_Comment by @kbknapp on 2016-05-30 04:33_

Yeah this should only be a single newline and not a double.

After usage, do you mean in the help message or elsewhere?


---

_Label `T: bug` added by @kbknapp on 2016-05-30 04:34_

---

_Label `P2: need to have` added by @kbknapp on 2016-05-30 04:34_

---

_Label `C: help message` added by @kbknapp on 2016-05-30 04:34_

---

_Label `D: easy` added by @kbknapp on 2016-05-30 04:34_

---

_Label `W: 2.x` added by @kbknapp on 2016-05-30 04:34_

---

_Referenced in [clap-rs/clap#517](../../clap-rs/clap/pulls/517.md) on 2016-05-30 10:23_

---

_Comment by @kbknapp on 2016-05-30 10:24_

#517 should take care of this, except the usage portion. Just waiting to get some more clarification on that part.


---

_Comment by @mitsuhiko on 2016-06-12 09:41_

@kbknapp basically every single output i get from clap (be it a default error from bad usage) or --help etc. includes an additional newline.


---

_Comment by @mitsuhiko on 2016-06-12 09:43_

This seems to be fixed now. Thanks a bunch!


---

_Closed by @kbknapp on 2016-06-12 19:30_

---

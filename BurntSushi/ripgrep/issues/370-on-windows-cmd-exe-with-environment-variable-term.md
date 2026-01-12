```yaml
number: 370
title: On Windows cmd.exe with environment variable TERM=msys, --color prints escape sequences
type: issue
state: closed
author: mgr2000
labels:
  - invalid
  - question
assignees: []
created_at: 2017-02-20T08:44:59Z
updated_at: 2017-02-21T13:22:03Z
url: https://github.com/BurntSushi/ripgrep/issues/370
synced_at: 2026-01-12T16:13:21Z
```

# On Windows cmd.exe with environment variable TERM=msys, --color prints escape sequences

---

_@mgr2000_

example output:
`[m[32m845[m:[m[31m[1mextern template[m struct BasicData<void>;`

coloring works fine after I do
_set TERM=_

This issue occurred after upgrading from ripgrep v 0.3.x to 0.4.0

---

_Comment by @BurntSushi on 2017-02-20 11:48_

This sounds like correct behavior. Why do you have `TERM=msys` in the native Windows console? `cmd.exe` is certainly not a MSYS terminal as far as I know.

---

_Label `question` added by @BurntSushi on 2017-02-20 11:48_

---

_Comment by @mgr2000 on 2017-02-21 13:20_

Thanks for the hint.
I suppose it was because TERM=NTCONSOLE leads to git complaining about "terminal is not fully functional" and some stackoverflow solution was to set TERM to msys instead...
Please consider this issue invalid.

---

_Closed by @mgr2000 on 2017-02-21 13:20_

---

_Label `invalid` added by @BurntSushi on 2017-02-21 13:22_

---

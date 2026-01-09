---
number: 176
title: Allow arguments to conflict in a POSIX compatible manner
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
assignees: []
created_at: 2015-08-16T19:10:31Z
updated_at: 2018-08-02T03:29:41Z
url: https://github.com/clap-rs/clap/issues/176
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow arguments to conflict in a POSIX compatible manner

---

_Issue opened by @kbknapp on 2015-08-16 19:10_

Take `ls` for example. `ls -C` is the column display mode, where as `ls -l` is the long listing display mode. The two are obviously not compatible. In POSIX, whichever argument comes _last_ wins, so to speak. Meaning, `ls -l -C` uses the default style, whereas `ls -C -l` uses the long listing format. In essence, the previous conflicting arguments are "forgotten."

This is why things like `alias ls='ls --some-option` works, because if it conflicts with what is actually passed, then the user is in effect over-riding the alias.

In `clap` you can't currently do this. You could declare `-l` and `-C` as conflicting, which would be parsing would fail if both are present (not allowing the aliasing). Or you could allow both, but there is no way to check which one came _last_.

This issue proposes adding a `.posix_conflicts_with("some arg")` (or similar name if someone has a better idea...`soft_conflicts_with()`, `conflicts_with_overridable()`, `overrideable()`, `mutually_overrides()`, etc.) Actually I kind of like `mutually_overrides()`. Which does exactly exactly what conflicts with, but instead of failing parsing, simply drops whichever arg it's conflicting with.

This way users can pick if they want POSIX style, regular conflicting arguments, or both.


---

_Label `T: new feature` added by @kbknapp on 2015-08-16 19:10_

---

_Label `P4: nice to have` added by @kbknapp on 2015-08-16 19:10_

---

_Label `D: easy` added by @kbknapp on 2015-08-16 19:10_

---

_Label `C: parsing` added by @kbknapp on 2015-08-16 19:10_

---

_Referenced in [clap-rs/clap#177](../../clap-rs/clap/pulls/177.md) on 2015-08-18 20:10_

---

_Comment by @kbknapp on 2015-08-20 01:24_

Closed with #177 


---

_Closed by @kbknapp on 2015-08-20 01:24_

---

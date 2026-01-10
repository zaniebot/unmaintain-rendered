---
number: 156
title: Allow version-less subcommands
type: issue
state: closed
author: kbknapp
labels: []
assignees: []
created_at: 2015-07-15T03:42:14Z
updated_at: 2018-08-02T03:29:41Z
url: https://github.com/clap-rs/clap/issues/156
synced_at: 2026-01-10T01:26:24Z
---

# Allow version-less subcommands

---

_Issue opened by @kbknapp on 2015-07-15 03:42_

Thanks to [SShrike](https://github.com/SShrike) for pointing this out!

Most `git`-like CLIs don't implement version on a per-subcommand basis. Granted there are times when you may wish to do so. But having the option to disable the versions for all but the top level command easily may also be desired.

This issues requests adding an `App::versionless_subcommands(bool)` which can disable all version below the current command in the hierarchy.


---

_Label `feature request` added by @kbknapp on 2015-07-15 03:42_

---

_Referenced in [clap-rs/clap#157](../../clap-rs/clap/issues/157.md) on 2015-07-15 03:45_

---

_Label `C: subcommands` added by @kbknapp on 2015-07-15 04:17_

---

_Label `D: easy` added by @kbknapp on 2015-07-15 04:17_

---

_Label `P3: want to have` added by @kbknapp on 2015-07-15 04:17_

---

_Closed by @kbknapp on 2015-07-16 05:43_

---

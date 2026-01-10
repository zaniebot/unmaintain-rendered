---
number: 307
title: "UnifiedHelpMessage doesn't sort"
type: issue
state: closed
author: birkenfeld
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2015-10-05T19:17:01Z
updated_at: 2018-08-02T03:29:46Z
url: https://github.com/clap-rs/clap/issues/307
synced_at: 2026-01-10T01:26:27Z
---

# UnifiedHelpMessage doesn't sort

---

_Issue opened by @birkenfeld on 2015-10-05 19:17_

With the UnifiedHelpMessage, the options are still listed in the same order as they would otherwise, just without headings.

For a unified options section, I would expect them to be completely alphabetically sorted.


---

_Comment by @kbknapp on 2015-10-05 22:56_

Thanks for reporting this :+1:  This is a "bug" (i.e. an oversight in implementation). `Args` (positional arguments) and `Subcommands` are still supposed to be broken out in separate categories, but `Options` and `Flags` are supposed to be combined into one category and sorted as a single category.


---

_Label `T: bug` added by @kbknapp on 2015-10-05 22:56_

---

_Label `P2: need to have` added by @kbknapp on 2015-10-05 22:56_

---

_Label `C: help message` added by @kbknapp on 2015-10-05 22:56_

---

_Label `D: easy` added by @kbknapp on 2015-10-05 22:56_

---

_Label `W: 1.x` added by @kbknapp on 2015-10-05 22:56_

---

_Added to milestone `1.4.4` by @kbknapp on 2015-10-05 22:56_

---

_Comment by @kbknapp on 2015-10-06 00:39_

@birkenfeld #311 fixes this, thanks again!


---

_Closed by @kbknapp on 2015-10-06 03:35_

---

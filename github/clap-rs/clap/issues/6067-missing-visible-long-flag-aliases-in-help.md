---
number: 6067
title: Missing visible long flag aliases in help
type: issue
state: closed
author: GilShoshan94
labels:
  - C-bug
  - A-help
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2025-07-09T21:47:44Z
updated_at: 2025-07-30T02:16:25Z
url: https://github.com/clap-rs/clap/issues/6067
synced_at: 2026-01-07T13:12:20-06:00
---

# Missing visible long flag aliases in help

---

_Issue opened by @GilShoshan94 on 2025-07-09 21:47_

I just found something strange, in this function `sc_spec_vals` for the aliases of the Command, I see we have 3 types:
- **aliases** -> and we show them in the help with `.get_visible_aliases()`
- **short_flag_aliases** -> with `.get_visible_short_flag_aliases()`
- **long_flag_aliases** -> with `.get_visible_long_flag_aliases()`

The visible aliases and the short flag ones are collected, but the long flag aliases are forgotten.

_Originally posted by @GilShoshan94 in https://github.com/clap-rs/clap/pull/6057#discussion_r2182025630_
            

---

_Referenced in [clap-rs/clap#6057](../../clap-rs/clap/pulls/6057.md) on 2025-07-09 21:49_

---

_Comment by @GilShoshan94 on 2025-07-09 21:51_

This was overlooked in #1974 and it is taken care of in PR #6068 as a minor fix in a commit

---

_Label `A-help` added by @epage on 2025-07-09 22:08_

---

_Label `C-bug` added by @epage on 2025-07-09 22:08_

---

_Label `E-help-wanted` added by @epage on 2025-07-09 22:08_

---

_Label `E-easy` added by @epage on 2025-07-09 22:08_

---

_Referenced in [clap-rs/clap#6068](../../clap-rs/clap/pulls/6068.md) on 2025-07-09 23:01_

---

_Closed by @epage on 2025-07-30 02:16_

---

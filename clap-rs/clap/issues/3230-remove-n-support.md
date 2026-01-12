```yaml
number: 3230
title: "Remove `{n}` support"
type: issue
state: open
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-help
  - S-blocked
assignees: []
created_at: 2021-12-30T17:44:37Z
updated_at: 2022-06-01T21:12:53Z
url: https://github.com/clap-rs/clap/issues/3230
synced_at: 2026-01-12T16:14:14Z
```

# Remove `{n}` support

---

_@epage_

Maintainer's notes
- Blocked on #2389
---

We previously removed `{n}` support in #1810 but found there wasn't a way to force a hard break without using `verbatim_doc_comment` which has downsides (#3198).  #2389 should address this but we don't want to block clap's development.

When we remove `{n}` support again, we should add asserts for it to help users catch the breaking change, like we did in 61c9e6265.

---

_Label `C-bug` added by @epage on 2021-12-30 17:44_

---

_Label `M-breaking-change` added by @epage on 2021-12-30 17:44_

---

_Label `A-help` added by @epage on 2021-12-30 17:44_

---

_Label `S-blocked` added by @epage on 2021-12-30 17:44_

---

_Added to milestone `4.0` by @epage on 2021-12-30 17:44_

---

_Referenced in [clap-rs/clap#3231](../../clap-rs/clap/pulls/3231.md) on 2021-12-30 17:45_

---

_Removed from milestone `4.0` by @epage on 2022-06-01 21:12_

---

_Added to milestone `5.0` by @epage on 2022-06-01 21:12_

---

_Referenced in [clap-rs/clap#4554](../../clap-rs/clap/issues/4554.md) on 2022-12-15 01:29_

---

_Referenced in [clap-rs/clap#5196](../../clap-rs/clap/issues/5196.md) on 2023-11-06 19:52_

---

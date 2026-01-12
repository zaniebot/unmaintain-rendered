```yaml
number: 3824
title: "Improve `clap_derive` error/warnings by always adding spans"
type: issue
state: open
author: epage
labels:
  - C-bug
  - E-easy
  - A-derive
assignees: []
created_at: 2022-06-13T19:36:49Z
updated_at: 2022-10-02T09:48:44Z
url: https://github.com/clap-rs/clap/issues/3824
synced_at: 2026-01-12T16:14:15Z
```

# Improve `clap_derive` error/warnings by always adding spans

---

_@epage_

In another issue, a user pointed out a [misattributing of a warning](https://github.com/clap-rs/clap/issues/3822#issuecomment-1154342341).  Sometimes, they'll even point to doc comments!

It is too easy to just use `quote!` and get a sub-bar experience for any warning or error generated in that code.  We should look into almost exclusively using `quote_spanned!`

---

_Label `C-bug` added by @epage on 2022-06-13 19:36_

---

_Label `E-easy` added by @epage on 2022-06-13 19:36_

---

_Label `A-derive` added by @epage on 2022-06-13 19:36_

---

_Referenced in [clap-rs/clap#3825](../../clap-rs/clap/pulls/3825.md) on 2022-06-13 21:15_

---

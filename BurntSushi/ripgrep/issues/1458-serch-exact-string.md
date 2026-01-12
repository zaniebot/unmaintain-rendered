```yaml
number: 1458
title: serch exact string
type: issue
state: closed
author: OrionRandD
labels:
  - invalid
assignees: []
created_at: 2020-01-11T15:10:32Z
updated_at: 2020-01-11T15:18:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1458
synced_at: 2026-01-12T16:13:23Z
```

# serch exact string

---

_@OrionRandD_

How do I search an exact string in a text.
If I

rg "casa"  file.md

It gives me: casados, acasalamento, etc...

But, I only want it to search the exact string casa (which is house in English)


---

_Comment by @BurntSushi on 2020-01-11 15:18_

Please consider exploring the `-w/--word-regexp` and `-x/--line-regexp` flags.

---

_Closed by @BurntSushi on 2020-01-11 15:18_

---

_Label `invalid` added by @BurntSushi on 2020-01-11 15:18_

---

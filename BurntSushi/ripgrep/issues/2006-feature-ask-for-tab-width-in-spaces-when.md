```yaml
number: 2006
title: "[Feature] Ask for tab width in spaces when calculating the search result column position"
type: issue
state: closed
author: pidgeon777
labels:
  - wontfix
assignees: []
created_at: 2021-09-28T12:21:58Z
updated_at: 2021-09-28T12:44:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2006
synced_at: 2026-01-12T16:13:24Z
```

# [Feature] Ask for tab width in spaces when calculating the search result column position

---

_@pidgeon777_

Actually, given a searched string, `ripgrep` can return:

- line position
- column position (in bytes, 1 character = 1 byte)

Please add an argument so that the column position is returned keeping into account the eventual presence of tabs, for example by asking the user how many spaces (bytes) should be associated with the tab character(s).


---

_Label `wontfix` added by @BurntSushi on 2021-09-28 12:44_

---

_Comment by @BurntSushi on 2021-09-28 12:44_

I don't think this is a good fit for ripgrep. I'd suggest post-processing the output somehow.

---

_Closed by @BurntSushi on 2021-09-28 12:44_

---

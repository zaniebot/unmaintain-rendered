```yaml
number: 877
title: Fall back to extension when file type pattern not defined?
type: issue
state: closed
author: mqudsi
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-04-03T00:22:16Z
updated_at: 2018-04-03T00:33:10Z
url: https://github.com/BurntSushi/ripgrep/issues/877
synced_at: 2026-01-12T16:13:22Z
```

# Fall back to extension when file type pattern not defined?

---

_@mqudsi_

I know that it is possible to "define" a (non-persistent) file type for use with `-t`, but what about falling back (with a possible warning message, if you would so prefer) to interpreting `-t EXT` as `-g '\.EXT$'` where EXT is not understood by `rg`?

---

_Comment by @BurntSushi on 2018-04-03 00:33_

No. I'd rather explicit failure modes rather than implicit guess-what-the-user-meant modes.

---

_Closed by @BurntSushi on 2018-04-03 00:33_

---

_Label `question` added by @BurntSushi on 2018-04-03 00:33_

---

_Label `wontfix` added by @BurntSushi on 2018-04-03 00:33_

---

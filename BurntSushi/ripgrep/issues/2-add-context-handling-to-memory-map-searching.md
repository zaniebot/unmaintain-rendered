```yaml
number: 2
title: add context handling to memory map searching
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2016-09-09T16:36:26Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2
synced_at: 2026-01-12T16:13:21Z
```

# add context handling to memory map searching

---

_@BurntSushi_

The memory map searcher supports every option except for printing contexts. Specifically, if any of `-A`, `-B` or `-C` are provided, then memory map searching can't be used.

Incidentally, handling contexts in the memory map searcher should be much easier than in the streaming searcher.


---

_Label `enhancement` added by @BurntSushi on 2016-09-09 16:36_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

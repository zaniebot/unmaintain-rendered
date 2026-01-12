```yaml
number: 947
title: Can you add a new tag after v0.8.1 (2/21 release)?  So we can get the -b (byte offset) flag?
type: issue
state: closed
author: ahungry
labels: []
assignees: []
created_at: 2018-06-14T01:15:39Z
updated_at: 2018-06-14T10:53:56Z
url: https://github.com/BurntSushi/ripgrep/issues/947
synced_at: 2026-01-12T16:13:22Z
```

# Can you add a new tag after v0.8.1 (2/21 release)?  So we can get the -b (byte offset) flag?

---

_@ahungry_

Currently one must clone the repo and compile it (and produce a binary labelled as v0.8.1 as a result) to get the byte offset flag - however a cargo install of v0.8.1 or Arch Linux install are both missing this option.

---

_Comment by @BurntSushi on 2018-06-14 10:53_

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

There is intentionally no release schedule. It happens when it happens.

> Currently one must clone the repo and compile it

`cargo install --git https://github.com/BurntSushi/ripgrep` will build and install from master.

---

_Closed by @BurntSushi on 2018-06-14 10:53_

---

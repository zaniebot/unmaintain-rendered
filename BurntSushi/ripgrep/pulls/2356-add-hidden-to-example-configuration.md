```yaml
number: 2356
title: "add '--hidden' to example configuration"
type: pull_request
state: closed
author: kevinushey
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2022-11-20T20:35:22Z
updated_at: 2023-07-10T15:59:06Z
url: https://github.com/BurntSushi/ripgrep/pull/2356
synced_at: 2026-01-12T18:23:14Z
```

# add '--hidden' to example configuration

---

_@kevinushey_

Motivated by issues like:

https://github.com/BurntSushi/ripgrep/issues/623

I understand (and agree with) the decision to not change the defaults; this just presents the commonly-used:

```
--hidden
--glob=!.git/*
```

as a potential "default configuration" for users who want `rg` to scan dotfiles, and hopefully makes that more discoverable.

---

_@BurntSushi approved on 2023-07-08 12:54_

Fair enough.

---

_Label `rollup` added by @BurntSushi on 2023-07-08 12:54_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Comment by @kevinushey on 2023-07-10 15:59_

Thank you!

---

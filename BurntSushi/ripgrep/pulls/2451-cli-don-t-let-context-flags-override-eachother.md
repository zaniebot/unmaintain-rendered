```yaml
number: 2451
title: "cli: don't let context flags override eachother"
type: pull_request
state: closed
author: r2p2
labels:
  - rollup
assignees: []
base: master
head: feature/context-2288
created_at: 2023-03-13T20:41:01Z
updated_at: 2023-07-08T22:52:52Z
url: https://github.com/BurntSushi/ripgrep/pull/2451
synced_at: 2026-01-12T18:23:14Z
```

# cli: don't let context flags override eachother

---

_@r2p2_

Match the behavior of GNU grep which does not ignore before-context and after-context completely if the context flag is also provided.

Fixes #2288

---

_Label `rollup` added by @BurntSushi on 2023-07-07 14:59_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

```yaml
number: 3182
title: "printer: fix panic in replacements in look-around corner case"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-printer-panic
created_at: 2025-10-12T20:31:23Z
updated_at: 2025-10-12T21:25:21Z
url: https://github.com/BurntSushi/ripgrep/pull/3182
synced_at: 2026-01-12T18:23:15Z
```

# printer: fix panic in replacements in look-around corner case

---

_@BurntSushi_

The abstraction boundary fuck up is the gift that keeps on giving. It
turns out that the invariant that the match would never exceed the range
given is not always true. So we kludge around it.

Fixes #3180


---

_Merged by @BurntSushi on 2025-10-12 21:25_

---

_Closed by @BurntSushi on 2025-10-12 21:25_

---

_Branch deleted on 2025-10-12 21:25_

---

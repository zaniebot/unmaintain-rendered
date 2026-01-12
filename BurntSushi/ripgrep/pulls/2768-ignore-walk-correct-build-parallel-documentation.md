```yaml
number: 2768
title: "ignore/walk: correct build_parallel() documentation"
type: pull_request
state: merged
author: cgzones
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2024-03-27T18:24:16Z
updated_at: 2024-03-27T19:02:18Z
url: https://github.com/BurntSushi/ripgrep/pull/2768
synced_at: 2026-01-12T18:23:14Z
```

# ignore/walk: correct build_parallel() documentation

---

_@cgzones_

The returned closure should return `WalkState`, not `()`.

Closes: #2767

---

_Review comment by @BurntSushi on `crates/ignore/src/walk.rs`:595 on 2024-03-27 18:31_

Can you use `println!("{path:?}")` to shorten things? And put it on one line? Even if it exceeds my typical line length limit, I'd rather that because I've found newlines inside of inline code fences to sometimes produce weird rendering behavior.

---

_@BurntSushi requested changes on 2024-03-27 18:31_

---

_@cgzones reviewed on 2024-03-27 18:36_

---

_Review comment by @cgzones on `crates/ignore/src/walk.rs`:595 on 2024-03-27 18:36_

Applied

---

_@BurntSushi approved on 2024-03-27 18:49_

---

_Merged by @BurntSushi on 2024-03-27 18:50_

---

_Closed by @BurntSushi on 2024-03-27 18:50_

---

_Branch deleted on 2024-03-27 19:02_

---

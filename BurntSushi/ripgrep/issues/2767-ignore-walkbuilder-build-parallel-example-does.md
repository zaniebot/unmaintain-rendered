```yaml
number: 2767
title: "ignore: WalkBuilder::build_parallel() example does not compile"
type: issue
state: closed
author: cgzones
labels: []
assignees: []
created_at: 2024-03-27T18:15:03Z
updated_at: 2024-03-27T18:50:06Z
url: https://github.com/BurntSushi/ripgrep/issues/2767
synced_at: 2026-01-12T16:13:24Z
```

# ignore: WalkBuilder::build_parallel() example does not compile

---

_@cgzones_

The example given in the documentation for `ignore::WalkBuildeer::build_parallel()`
https://github.com/BurntSushi/ripgrep/blob/eca13f08a2a0ece89a423565891e48a90a8aedef/crates/ignore/src/walk.rs#L594
does not compile, since the returned closure is expected to return a `WalkState`, not `()`.

---

_Comment by @BurntSushi on 2024-03-27 18:16_

PRs are welcome to fix this sort of thing.

---

_Closed by @BurntSushi on 2024-03-27 18:50_

---

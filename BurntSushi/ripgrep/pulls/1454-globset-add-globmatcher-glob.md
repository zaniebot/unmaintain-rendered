```yaml
number: 1454
title: "globset: Add GlobMatcher::glob"
type: pull_request
state: closed
author: LPGhatguy
labels:
  - rollup
assignees: []
base: master
head: expose-globmatcher-glob
created_at: 2020-01-03T23:41:07Z
updated_at: 2020-02-18T18:47:52Z
url: https://github.com/BurntSushi/ripgrep/pull/1454
synced_at: 2026-01-12T18:23:13Z
```

# globset: Add GlobMatcher::glob

---

_@LPGhatguy_

The `GlobMatcher` type holds onto the `Glob` object that was used to create it. This PR exposes a reference to it through a getter.

My main use is that I have a project that uses globset by wrapping the `GlobMatcher` type in a type that I can implement Serde's `Serialize` and `Deserialize` on. In order to get access to the original pattern, I [currently need to redundantly keep my own copy of the `Glob` struct](https://github.com/rojo-rbx/rojo/blob/c82bc8a3583a65279d7aa28cde4b53977cd78fc4/src/glob.rs) to get access to its string pattern.

This API would enable me to only need to keep around `GlobMatcher`.

---

_Comment by @LPGhatguy on 2020-01-03 23:55_

CI seems to have failed on a handful of things being broken in other places. 🙁 

---

_Label `rollup` added by @BurntSushi on 2020-02-17 00:44_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Branch deleted on 2020-02-18 18:47_

---

_Comment by @LPGhatguy on 2020-02-18 18:47_

Thanks!

---

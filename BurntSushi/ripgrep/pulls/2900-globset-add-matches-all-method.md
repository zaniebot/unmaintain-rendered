```yaml
number: 2900
title: "globset: add matches_all method"
type: pull_request
state: closed
author: tmccombs
labels:
  - rollup
assignees: []
base: master
head: matches-all
created_at: 2024-09-19T07:35:22Z
updated_at: 2025-09-20T01:46:07Z
url: https://github.com/BurntSushi/ripgrep/pull/2900
synced_at: 2026-01-12T18:23:14Z
```

# globset: add matches_all method

---

_@tmccombs_

This returns true if all globs in the set match the supplied file.

Fixes: #2869

---

_Comment by @tmccombs on 2024-09-19 07:55_

See also https://github.com/rust-lang/regex/pull/1228

---

_Review comment by @BurntSushi on `crates/globset/src/lib.rs`:354 on 2024-09-19 13:34_

I don't think we need to use caps for `ALL` here. Just "all" is fine.

---

_Review comment by @BurntSushi on `crates/globset/src/lib.rs`:365 on 2024-09-19 13:39_

Please wrap lines to 79 columns (inclusive).

---

_Review comment by @BurntSushi on `crates/globset/src/lib.rs`:365 on 2024-09-19 13:39_

Also, this note should be added to `matches_all` too.

---

_@BurntSushi requested changes on 2024-09-19 13:41_

---

_Label `rollup` added by @BurntSushi on 2025-07-26 15:45_

---

_@BurntSushi approved on 2025-07-26 15:45_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Branch deleted on 2025-09-20 01:46_

---

```yaml
number: 2539
title: Fix rust-version in Cargo.toml
type: pull_request
state: closed
author: jschwe
labels:
  - rollup
assignees: []
base: master
head: patch-1
created_at: 2023-06-19T09:20:01Z
updated_at: 2023-07-09T07:40:15Z
url: https://github.com/BurntSushi/ripgrep/pull/2539
synced_at: 2026-01-12T18:23:14Z
```

# Fix rust-version in Cargo.toml

---

_@jschwe_

ripgrep requires Rust 1.70, which is already documented in the Readme. This change adjusts the Cargo.toml to reflect this.

---

_Comment by @ltrzesniewski on 2023-06-19 09:28_

There's still a mention of Rust 1.65 in the readme BTW (in the *Building* section), you could fix that in this PR as well. 🙂

---

_@memark approved on 2023-07-03 07:03_

LGTM

---

_Label `rollup` added by @BurntSushi on 2023-07-05 18:25_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Branch deleted on 2023-07-09 07:40_

---

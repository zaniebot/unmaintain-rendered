```yaml
number: 3151
title: "fix: clippy errors"
type: pull_request
state: merged
author: ltrzesniewski
labels: []
assignees: []
merged: true
base: master
head: fix-clippy-errors
created_at: 2025-09-20T13:45:05Z
updated_at: 2025-09-21T13:17:12Z
url: https://github.com/BurntSushi/ripgrep/pull/3151
synced_at: 2026-01-12T18:23:15Z
```

# fix: clippy errors

---

_@ltrzesniewski_

I opened the project in RustRover on Windows after you merged the rollup branch, and it showed a few Clippy errors. Those were displayed by highlighting a few files in red, which is annoying, so I thought I'd rather get rid of them.

This is basically the equivalent of handling the output of `cargo clippy -- -A warnings` with Rust 1.90 on Windows. (Clippy also shows 72 warnings without `-A warnings`, 60 of whose can be auto-applied, but I thought you wouldn't want a PR like that).

RustRover now only complains about the `#![feature(test)]` in the `globset` bench, but I don't think I can do anything about that without changing the toolchain or using another benching framework.



---

_@BurntSushi approved on 2025-09-21 13:15_

Thanks!

---

_Merged by @BurntSushi on 2025-09-21 13:15_

---

_Closed by @BurntSushi on 2025-09-21 13:15_

---

_Branch deleted on 2025-09-21 13:17_

---

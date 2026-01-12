```yaml
number: 447
title: "Fix import of `CacheArgs` in `puffin-cli`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/fix-cache
created_at: 2023-11-17T20:23:06Z
updated_at: 2023-11-17T20:35:02Z
url: https://github.com/astral-sh/uv/pull/447
synced_at: 2026-01-12T16:03:57Z
```

# Fix import of `CacheArgs` in `puffin-cli`

---

_@zanieb_

```
error[E0432]: unresolved imports `puffin_cache::CacheArgs`, `puffin_cache::CacheDir`
  --> crates/puffin-cli/src/main.rs:11:20
   |
11 | use puffin_cache::{CacheArgs, CacheDir};
   |                    ^^^^^^^^^  ^^^^^^^^ no `CacheDir` in the root
   |                    |
   |                    no `CacheArgs` in the root
   |
note: found an item that was configured out
  --> /Users/mz/eng/src/astral-sh/puffin/crates/puffin-cache/src/lib.rs:7:15
   |
7  | pub use cli::{CacheArgs, CacheDir};
   |               ^^^^^^^^^
   = note: the item is gated behind the `clap` feature
note: found an item that was configured out
  --> /Users/mz/eng/src/astral-sh/puffin/crates/puffin-cache/src/lib.rs:7:26
   |
7  | pub use cli::{CacheArgs, CacheDir};
   |                          ^^^^^^^^
   = note: the item is gated behind the `clap` feature

For more information about this error, try `rustc --explain E0432`.
error: could not compile `puffin-cli` (bin "puffin") due to previous error
```

---

_@charliermarsh approved on 2023-11-17 20:27_

---

_Comment by @charliermarsh on 2023-11-17 20:31_

Thanks, sorry!

---

_Merged by @charliermarsh on 2023-11-17 20:35_

---

_Closed by @charliermarsh on 2023-11-17 20:35_

---

_Branch deleted on 2023-11-17 20:35_

---

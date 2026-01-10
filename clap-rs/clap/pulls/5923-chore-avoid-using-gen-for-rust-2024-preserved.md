---
number: 5923
title: "chore: Avoid using `gen` for rust 2024 preserved keyword"
type: pull_request
state: merged
author: Reverier-Xu
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2025-02-21T16:42:32Z
updated_at: 2025-02-24T15:24:09Z
url: https://github.com/clap-rs/clap/pull/5923
synced_at: 2026-01-10T01:28:25Z
---

# chore: Avoid using `gen` for rust 2024 preserved keyword

---

_Pull request opened by @Reverier-Xu on 2025-02-21 16:42_

In the rust 2024 edition, [`gen` has been a reserved keyword](https://doc.rust-lang.org/edition-guide/rust-2024/gen-keyword.html), so we should avoid using it.

---

_@billf reviewed on 2025-02-21 20:51_

---

_Review comment by @billf on `clap_complete/src/aot/generator/mod.rs`:266 on 2025-02-21 20:51_

`gen: G`

---

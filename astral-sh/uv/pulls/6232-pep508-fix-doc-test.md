```yaml
number: 6232
title: "pep508: fix doc test"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-pep508-doctest
created_at: 2024-08-19T21:25:28Z
updated_at: 2024-08-19T23:29:22Z
url: https://github.com/astral-sh/uv/pull/6232
synced_at: 2026-01-12T16:07:17Z
```

# pep508: fix doc test

---

_@BurntSushi_

Indented blocks in Markdown are treated as code blocks, and rustdoc
treats all unadorned code blocks as Rust doctests. Since this wasn't
intended as a doctest and isn't valid Rust, it makes `cargo test --doc`
fail. We fix this by using an explicit code block labeled as `text`.


---

_Review requested from @zanieb by @BurntSushi on 2024-08-19 21:25_

---

_@zanieb approved on 2024-08-19 21:27_

Sorry!

---

_@zanieb reviewed on 2024-08-19 21:27_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/parse.rs`:261 on 2024-08-19 21:27_

Maybe doesn't need to be indented then?

---

_@BurntSushi reviewed on 2024-08-19 21:31_

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/parse.rs`:261 on 2024-08-19 21:31_

Aye. And I caught another one in `uv-resolver`.

---

_Merged by @BurntSushi on 2024-08-19 23:29_

---

_Closed by @BurntSushi on 2024-08-19 23:29_

---

_Branch deleted on 2024-08-19 23:29_

---

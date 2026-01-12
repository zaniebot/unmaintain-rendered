```yaml
number: 5011
title: "uv: fix doc test"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fix-doc-test
created_at: 2024-07-12T14:47:33Z
updated_at: 2024-07-12T14:57:31Z
url: https://github.com/astral-sh/uv/pull/5011
synced_at: 2026-01-12T16:06:35Z
```

# uv: fix doc test

---

_@BurntSushi_

[Doc tests can't use crate internal APIs unfortunately.][internal-doc]
I really want them to be able to, but for now, mark such tests as
`ignore`.

I was motivated to do this because it otherwise breaks `cargo t --all`
for me.

[internal-doc]: https://github.com/rust-lang/rust/issues/50784


---

_@konstin approved on 2024-07-12 14:54_

---

_Label `internal` added by @zanieb on 2024-07-12 14:56_

---

_Merged by @BurntSushi on 2024-07-12 14:57_

---

_Closed by @BurntSushi on 2024-07-12 14:57_

---

_Branch deleted on 2024-07-12 14:57_

---

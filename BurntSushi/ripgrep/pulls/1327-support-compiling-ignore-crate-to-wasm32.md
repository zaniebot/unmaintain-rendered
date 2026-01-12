```yaml
number: 1327
title: "Support compiling `ignore` crate to wasm32"
type: pull_request
state: merged
author: tiziano88
labels: []
assignees: []
merged: true
base: master
head: tzn_wasm
created_at: 2019-07-24T12:58:21Z
updated_at: 2019-07-24T16:56:26Z
url: https://github.com/BurntSushi/ripgrep/pull/1327
synced_at: 2026-01-12T18:23:13Z
```

# Support compiling `ignore` crate to wasm32

---

_@tiziano88_

Currently the crate assumes that exactly one of `cfg(windows)` or
`cfg(unix)` is true, but this is not actually the case, for instance
when compiling for `wasm32`.

Implement the missing functions so that the crate can compile on other
platforms, even though those functions will always return an error.

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:439 on 2019-07-24 13:18_

I don't think `NotFound` is the right error kind for this. Can you use `Other` instead?

---

_@BurntSushi requested changes on 2019-07-24 13:19_

Thanks! Just one nit.

---

_@tiziano88 reviewed on 2019-07-24 15:15_

---

_Review comment by @tiziano88 on `ignore/src/walk.rs`:439 on 2019-07-24 15:15_

Done.

---

_Merged by @BurntSushi on 2019-07-24 16:55_

---

_Closed by @BurntSushi on 2019-07-24 16:55_

---

_Comment by @BurntSushi on 2019-07-24 16:56_

This PR is on crates.io in `ignore 0.4.8`.

---

---
number: 3332
title: "Documentation Error: `e.exit` now exits with code 2 as per #1327"
type: issue
state: closed
author: rlee287
labels: []
assignees: []
created_at: 2022-01-23T22:25:12Z
updated_at: 2022-01-24T14:11:34Z
url: https://github.com/clap-rs/clap/issues/3332
synced_at: 2026-01-10T01:27:39Z
---

# Documentation Error: `e.exit` now exits with code 2 as per #1327

---

_Issue opened by @rlee287 on 2022-01-23 22:25_

https://github.com/clap-rs/clap/blob/d52b326c7b9c6a033372148d39bce5d688ef8ba8/src/parse/errors.rs#L478-L500

As per #1327, the `USAGE_EXIT` code is now 2, but the documentation still states that it exits with code 1.

---

_Closed by @epage on 2022-01-24 14:11_

---

_Comment by @epage on 2022-01-24 14:11_

Thanks for reporting this!

---

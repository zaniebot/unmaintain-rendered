```yaml
number: 10958
title: Do not suggest --package when --backend is used, but --build-backend
type: pull_request
state: merged
author: issokuos
labels: []
assignees: []
merged: true
base: main
head: fix-8743
created_at: 2025-01-25T13:58:53Z
updated_at: 2025-01-30T06:04:32Z
url: https://github.com/astral-sh/uv/pull/10958
synced_at: 2026-01-10T11:10:34Z
```

# Do not suggest --package when --backend is used, but --build-backend

---

_Pull request opened by @issokuos on 2025-01-25 13:58_

## Summary

Closes #8743

## Test Plan

Snapshot tests


---

_Review requested from @zanieb by @charliermarsh on 2025-01-25 19:54_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/init.rs`:2266 on 2025-01-25 23:29_

This seems suboptimal. What happens if we make `--backend` a flag, so it doesn't have a value?

---

_@charliermarsh reviewed on 2025-01-25 23:29_

---

_@issokuos reviewed on 2025-01-26 00:00_

---

_Review comment by @issokuos on `crates/uv/tests/it/init.rs`:2266 on 2025-01-26 00:00_

I think that is better. I found this error message annoying  but didn't remember the flag alternative. The other alternative I thought about was making `--backend` an alias.

---

_Renamed from "Do not suggest --package when --backend is used, but --backend-build" to "Do not suggest --package when --backend is used, but --build-backend" by @issokuos on 2025-01-26 00:04_

---

_@zanieb reviewed on 2025-01-28 00:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:2266 on 2025-01-28 00:22_

Yeah I agree this is suboptimal. A flag might make sense but then what happens if you provide a value? 

Unfortunately this is sort of the status quo upstream, from what I understood in https://github.com/clap-rs/clap/discussions/5781#discussioncomment-11104839 

---

_@zanieb reviewed on 2025-01-28 00:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:2266 on 2025-01-28 00:23_

I don't really want to make it an alias, it'd be nice to just be able to point people to the canonical name in a way that doesn't cause a bunch of weird behaviors :(

---

_Review comment by @issokuos on `crates/uv/tests/it/init.rs`:2266 on 2025-01-28 10:53_

Making  this a flag doesn't seem to be an option since it breaks the `build-backend` option:(

---

_@issokuos reviewed on 2025-01-28 10:53_

---

_Comment by @zanieb on 2025-01-29 14:50_

Perhaps there's an example in Cargo's implementation we could use? Since they make heavy use of this pattern.

---

_Comment by @issokuos on 2025-01-29 19:29_




> Perhaps there's an example in Cargo's implementation we could use? Since they make heavy use of this pattern.

I used `ArgAction` like in Cargo to make it work

---

_@zanieb approved on 2025-01-29 22:14_

---

_Comment by @zanieb on 2025-01-29 22:15_

Thank you!

---

_Merged by @zanieb on 2025-01-29 22:15_

---

_Closed by @zanieb on 2025-01-29 22:15_

---

_Branch deleted on 2025-01-30 06:04_

---

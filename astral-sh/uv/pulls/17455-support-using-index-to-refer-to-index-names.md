```yaml
number: 17455
title: "Support using `--index` to refer to index names"
type: pull_request
state: open
author: EliteTK
labels: []
assignees: []
base: main
head: tk/index-by-name
created_at: 2026-01-13T23:35:35Z
updated_at: 2026-01-19T15:16:16Z
url: https://github.com/astral-sh/uv/pull/17455
synced_at: 2026-01-19T15:25:20Z
```

# Support using `--index` to refer to index names

---

_@EliteTK_

## Summary

Implement #13974.

WIP

## Test Plan

To Do

---

_@EliteTK reviewed on 2026-01-15 19:38_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11501 on 2026-01-15 19:38_

Need to investigate what happens if you don't pass `--index`.

---

_@EliteTK reviewed on 2026-01-15 19:39_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11283 on 2026-01-15 19:39_

I am not sure if we want this. But also, need to check what happens normally.

---

_@EliteTK reviewed on 2026-01-19 14:30_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11501 on 2026-01-19 14:30_

So, I am going to leave this as is for now.

Although I think there's an argument to be made that if you're adding an index and it matches identically an index in the workspace (regardless of if you used the name, or spelled it out) that maybe we shouldn't mirror it in the child.

But the way things like #17610 work seems like maybe justification for leaving it as is.

---

_@EliteTK reviewed on 2026-01-19 14:50_

---

_Review comment by @EliteTK on `crates/uv/tests/it/edit.rs`:11283 on 2026-01-19 14:50_

Okay, so index name references in `[tool.uv.sources]` are resolved in "project" then "workspace" order.

But when it comes to picking an index to use for a package without a specified source, we pick "workspace" then "project".

I'm not really sure what the correct answer is here. I'm going to leave it as is for now...

---

_Marked ready for review by @EliteTK on 2026-01-19 15:16_

---

_Review requested from @konstin by @EliteTK on 2026-01-19 15:16_

---

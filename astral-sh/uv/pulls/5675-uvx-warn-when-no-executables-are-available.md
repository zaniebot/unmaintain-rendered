```yaml
number: 5675
title: "`uvx` warn when no executables are available"
type: pull_request
state: merged
author: blueraft
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: uvx-no-exec-found
created_at: 2024-07-31T21:32:58Z
updated_at: 2024-07-31T22:27:41Z
url: https://github.com/astral-sh/uv/pull/5675
synced_at: 2026-01-10T13:37:23Z
```

# `uvx` warn when no executables are available

---

_Pull request opened by @blueraft on 2024-07-31 21:32_

## Summary

Resolves #5623 

## Test Plan

`cargo test`


---

_@zanieb reviewed on 2024-07-31 21:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:166 on 2024-07-31 21:46_

I'd leave the above as-is and in the new message say something like "warning: `{from.name}` does not provide any executables". Wdyt?

---

_Assigned to @zanieb by @zanieb on 2024-07-31 21:51_

---

_@blueraft reviewed on 2024-07-31 21:53_

---

_Review comment by @blueraft on `crates/uv/src/commands/tool/run.rs`:166 on 2024-07-31 21:53_

Yeah makes sense, I will push the change in a sec.

---

_@blueraft reviewed on 2024-07-31 21:56_

---

_Review comment by @blueraft on `crates/uv/src/commands/tool/run.rs`:166 on 2024-07-31 21:56_

Done

---

_@zanieb reviewed on 2024-07-31 22:02_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:866 on 2024-07-31 22:02_

🤔  We should probably display the second warning _instead_ of the first warning if we can. 

---

_@blueraft reviewed on 2024-07-31 22:16_

---

_Review comment by @blueraft on `crates/uv/tests/tool_run.rs`:866 on 2024-07-31 22:16_

Done

---

_@zanieb approved on 2024-07-31 22:25_

Thank you!

---

_Label `cli` added by @zanieb on 2024-07-31 22:25_

---

_Label `preview` added by @zanieb on 2024-07-31 22:25_

---

_Merged by @zanieb on 2024-07-31 22:27_

---

_Closed by @zanieb on 2024-07-31 22:27_

---

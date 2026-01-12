```yaml
number: 14135
title: "add support for `excluded-dependencies` "
type: pull_request
state: closed
author: CodeMan62
labels: []
assignees: []
draft: true
base: main
head: excluded-dependencies-support
created_at: 2025-06-18T18:43:59Z
updated_at: 2025-08-21T18:14:34Z
url: https://github.com/astral-sh/uv/pull/14135
synced_at: 2026-01-12T16:11:02Z
```

# add support for `excluded-dependencies` 

---

_@CodeMan62_

## Summary
This PR adds support for  `excluded-dependencies` in `pyproject.toml`

## Test Plan
I have added tests un `uv/tests` section
Closes #12616 

---

_Renamed from "add support for excluded-dependencies" to "add support for `excluded-dependencies` in `pyproject.toml`" by @CodeMan62 on 2025-06-18 18:47_

---

_Comment by @CodeMan62 on 2025-06-19 17:42_

cc @konstin @zanieb 

---

_Renamed from "add support for `excluded-dependencies` in `pyproject.toml`" to "add support for `excluded-dependencies` " by @CodeMan62 on 2025-06-22 17:33_

---

_Comment by @CodeMan62 on 2025-07-04 03:32_

can someone from team please have a look at it

---

_@zanieb reviewed on 2025-07-15 12:57_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:444 on 2025-07-15 12:57_

I'm not sure if we can / should use admonitions in these docs

---

_@zanieb reviewed on 2025-07-15 12:57_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:442 on 2025-07-15 12:57_

This seems confusing? Is this text copied from the constraints documentation? I don't think we need to explain that excluded dependencies are not installed.

---

_@zanieb reviewed on 2025-07-15 12:58_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:438 on 2025-07-15 12:58_

We should also note this excludes all of that package's dependencies.

---

_@zanieb reviewed on 2025-07-15 12:58_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:461 on 2025-07-15 12:58_

This comment does not match the example.

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:1767 on 2025-07-15 12:59_

It doesn't look like `excluded-dependencies` is working in your test

---

_@zanieb reviewed on 2025-07-15 12:59_

---

_Comment by @zanieb on 2025-07-15 13:00_

It doesn't look like you actually implemented `excluded-dependencies`? The option is there, but it's not propagated to the resolver.

---

_Converted to draft by @zanieb on 2025-07-15 13:00_

---

_Closed by @CodeMan62 on 2025-08-21 18:14_

---

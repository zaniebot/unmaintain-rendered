```yaml
number: 12381
title: "Add test case for `uv python list` downloads"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/list-tests-downloads
created_at: 2025-03-21T18:55:43Z
updated_at: 2025-04-21T22:24:56Z
url: https://github.com/astral-sh/uv/pull/12381
synced_at: 2026-01-10T11:10:39Z
```

# Add test case for `uv python list` downloads

---

_Pull request opened by @zanieb on 2025-03-21 18:55_

Requires #12380 
Extends new tests from #12374 

Is waiting for dependent PRs to merge; for early review see https://github.com/astral-sh/uv/pull/12381/commits/a27c93e3b686d13d472cc688a68e704478097beb

---

_Label `testing` added by @zanieb on 2025-03-21 18:55_

---

_@zanieb reviewed on 2025-03-21 18:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_list.rs`:143 on 2025-03-21 18:57_

This might not actually be cross-platform. Testing in CI.

---

_Comment by @zanieb on 2025-03-21 20:00_

Interesting, both Linux and Windows have a couple 3.7 versions

```
Snapshot: python_list_downloads
Source: crates/uv/tests/it/python_list.rs:143
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    8     8 │ cpython-3.11.11-[PLATFORM]                  <download available>
    9     9 │ cpython-3.10.16-[PLATFORM]                  <download available>
   10    10 │ cpython-3.9.21-[PLATFORM]                   <download available>
   11    11 │ cpython-3.8.20-[PLATFORM]                   <download available>
         12 │+cpython-3.7.9-[PLATFORM]                    <download available>
   12    13 │ pypy-3.11.11-[PLATFORM]                     <download available>
   13    14 │ pypy-3.10.16-[PLATFORM]                     <download available>
   14    15 │ pypy-3.9.19-[PLATFORM]                      <download available>
   15    16 │ pypy-3.8.16-[PLATFORM]                      <download available>
         17 │+pypy-3.7.13-[PLATFORM]                      <download available>
   16    18 │ 
   17    19 │ ----- stderr -----
────────────┴───────────────────────────────────────────────────────────────────
```

---

_@zanieb reviewed on 2025-03-21 21:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_list.rs`:143 on 2025-03-21 21:15_

It's not, so we'll filter to a subset of Python versions on top of #12375 

---

_@charliermarsh approved on 2025-03-21 22:36_

---

_Marked ready for review by @zanieb on 2025-04-21 22:16_

---

_Merged by @zanieb on 2025-04-21 22:24_

---

_Closed by @zanieb on 2025-04-21 22:24_

---

_Branch deleted on 2025-04-21 22:24_

---

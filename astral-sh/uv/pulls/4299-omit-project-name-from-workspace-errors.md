```yaml
number: 4299
title: Omit project name from workspace errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: charlie/project-name
created_at: 2024-06-13T02:44:27Z
updated_at: 2024-06-14T01:32:52Z
url: https://github.com/astral-sh/uv/pull/4299
synced_at: 2026-01-10T13:54:02Z
```

# Omit project name from workspace errors

---

_Pull request opened by @charliermarsh on 2024-06-13 02:44_

## Summary

Because the workspace member itself is part of the resolution, adding the workspace name for the project leads to confusing errors, like:

```
❯ cargo run lock --preview
   Compiling uv v0.2.11 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 1.79s
     Running `/Users/crmarsh/workspace/puffin/target/debug/uv lock --preview`
  × No solution found when resolving dependencies:
  ╰─▶ Because only albatross==0.1.0 is available and albatross==0.1.0 depends on anyio<=3, we can conclude that all versions of albatross depend on anyio<=3.
      And because bird-feeder==1.0.0 depends on anyio>=4.3.0,<5 and only bird-feeder==1.0.0 is available, we can conclude that all versions of albatross and all versions of bird-feeder are incompatible.
      And because albatross depends on albatross and bird-feeder, we can conclude that the requirements are unsatisfiable.
```

(Notice "albatross depends on albatross".)


---

_Label `error messages` added by @charliermarsh on 2024-06-13 02:44_

---

_Label `preview` added by @charliermarsh on 2024-06-13 02:44_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-13 02:44_

---

_Review requested from @konstin by @charliermarsh on 2024-06-13 02:44_

---

_@zanieb reviewed on 2024-06-13 13:11_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:105 on 2024-06-13 13:11_

Should we say... "The workspace `requires-python`field..."?

---

_@zanieb approved on 2024-06-14 01:23_

---

_Merged by @charliermarsh on 2024-06-14 01:32_

---

_Closed by @charliermarsh on 2024-06-14 01:32_

---

_Branch deleted on 2024-06-14 01:32_

---

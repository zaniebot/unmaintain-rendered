```yaml
number: 2702
title: Recursively allow URL requirements for local dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/recur
created_at: 2024-03-28T01:51:13Z
updated_at: 2024-03-28T22:53:58Z
url: https://github.com/astral-sh/uv/pull/2702
synced_at: 2026-01-10T14:49:08Z
```

# Recursively allow URL requirements for local dependencies

---

_Pull request opened by @charliermarsh on 2024-03-28 01:51_

## Summary

This is a trimmed-down version of https://github.com/astral-sh/uv/pull/2684 that only applies to local source trees for now, which enables workspace-like workflows (whereby local packages can depend on other local packages at arbitrary depth).

Closes #2699.

## Test Plan

Added new tests.

Also cloned this MRE that was shared with me (https://github.com/timothyjlaurent/uv-poetry-monorepo-mre), and verified that it was installed without error:

```
❯ cargo run pip install ./uv-poetry-monorepo-mre/app --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip install ./uv-poetry-monorepo-mre/app --no-cache`
Resolved 4 packages in 1.28s
   Built app @ file:///Users/crmarsh/workspace/uv/uv-poetry-monorepo-mre/app
   Built lib1 @ file:///Users/crmarsh/workspace/uv/uv-poetry-monorepo-mre/lib1
   Built lib2 @ file:///Users/crmarsh/workspace/uv/uv-poetry-monorepo-mre/lib2                                        Downloaded 4 packages in 457ms
Installed 4 packages in 2ms
 + app==0.1.0 (from file:///Users/crmarsh/workspace/uv/uv-poetry-monorepo-mre/app)
 + lib1==0.1.0 (from file:///Users/crmarsh/workspace/uv/uv-poetry-monorepo-mre/lib1)
 + lib2==0.1.0 (from file:///Users/crmarsh/workspace/uv/uv-poetry-monorepo-mre/lib2)
 + ruff==0.3.4
```


---

_Marked ready for review by @charliermarsh on 2024-03-28 02:03_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-28 02:03_

---

_Label `enhancement` added by @charliermarsh on 2024-03-28 02:12_

---

_Review requested from @konstin by @charliermarsh on 2024-03-28 13:40_

---

_@zanieb approved on 2024-03-28 21:16_

---

_Merged by @charliermarsh on 2024-03-28 22:53_

---

_Closed by @charliermarsh on 2024-03-28 22:53_

---

_Branch deleted on 2024-03-28 22:53_

---

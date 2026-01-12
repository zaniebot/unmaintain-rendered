```yaml
number: 12953
title: "Fix `uv python list --only-downloads --all-arches`"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: main
head: list-fix
created_at: 2025-04-17T21:33:47Z
updated_at: 2025-07-18T11:54:23Z
url: https://github.com/astral-sh/uv/pull/12953
synced_at: 2026-01-12T16:10:28Z
```

# Fix `uv python list --only-downloads --all-arches`

---

_@j178_

## Summary

This PR fixes a issue where using `--only-downloads` with `--all-arches`, `--all-arches` is not honored.

~Depends on #12931~


---

_Comment by @alexprengere on 2025-04-22 08:55_

I believe this issue is still present in `0.6.16`. It needs to be updated as tests were added in https://github.com/astral-sh/uv/pull/12381

---

_Review requested from @zanieb by @konstin on 2025-04-22 09:08_

---

_Assigned to @zanieb by @zanieb on 2025-04-22 12:34_

---

_Comment by @j178 on 2025-07-18 03:52_

Superseded by #14629 

---

_Closed by @j178 on 2025-07-18 03:52_

---

_Comment by @zanieb on 2025-07-18 11:43_

Sorry for merging a subsequent pull request — I'd lost track of this one.

---

_Branch deleted on 2025-07-18 11:46_

---

_Comment by @alexprengere on 2025-07-18 11:54_

It is actually my bad, I totally forgot about this PR, encountered the problem, then proceeded to open another PR with the fix.

---

```yaml
number: 1169
title: Cleanup puffin interpreter errors
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/cleanup-puffin-interpreter-errors
created_at: 2024-01-29T12:14:46Z
updated_at: 2024-01-30T09:52:47Z
url: https://github.com/astral-sh/uv/pull/1169
synced_at: 2026-01-12T16:04:29Z
```

# Cleanup puffin interpreter errors

---

_@konstin_

Use `virtualenv` consistently, remove unused error variants and hint the user towards installing missing python versions.

I didn't touch the Readme but i replaced `virtualenv environment` with `virtualenv` in the strings i found.

Fixes https://github.com/astral-sh/puffin/issues/1167

---

_Review requested from @MichaReiser by @konstin on 2024-01-29 12:14_

---

_Review requested from @charliermarsh by @konstin on 2024-01-29 12:14_

---

_@charliermarsh reviewed on 2024-01-29 12:46_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/lib.rs`:43 on 2024-01-29 12:46_

Can we do "Is Python {major}.{minor} installed?"

---

_@charliermarsh approved on 2024-01-29 12:46_

---

_Label `error messages` added by @charliermarsh on 2024-01-29 12:46_

---

_Comment by @zanieb on 2024-01-29 15:20_

Ah thanks I was going to do this :D

---

_@zanieb reviewed on 2024-01-29 15:21_

---

_Review comment by @zanieb on `crates/puffin-interpreter/src/python_query.rs`:164 on 2024-01-29 15:21_

I think we should special case and exclude this parent error, it doesn't add any more information and it's confusing

---

_@charliermarsh reviewed on 2024-01-29 15:51_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/python_query.rs`:164 on 2024-01-29 15:51_

Agree

---

_Comment by @konstin on 2024-01-29 16:48_

Current dependencies on/for this PR:
* `main`
  * **PR #1172** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1172?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #1169** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1169?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/1169?utm_source=stack-comment).

---

_Merged by @konstin on 2024-01-30 09:52_

---

_Closed by @konstin on 2024-01-30 09:52_

---

_Branch deleted on 2024-01-30 09:52_

---

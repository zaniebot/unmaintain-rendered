```yaml
number: 13486
title: Detect basic wildcard imports in ruff analyze graph
type: pull_request
state: merged
author: charliermarsh
labels:
  - analyze
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2024-09-23T14:07:32Z
updated_at: 2024-09-23T22:09:02Z
url: https://github.com/astral-sh/ruff/pull/13486
synced_at: 2026-01-10T20:59:36Z
```

# Detect basic wildcard imports in ruff analyze graph

---

_Pull request opened by @charliermarsh on 2024-09-23 14:07_

## Summary

I guess we can just ignore the `*` entirely for now? This will add the `__init__.py` for anything that's importing a package.


---

_Review requested from @carljm by @charliermarsh on 2024-09-23 14:07_

---

_Label `analyze` added by @charliermarsh on 2024-09-23 14:07_

---

_Comment by @github-actions[bot] on 2024-09-23 14:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (error)</summary>
<p>

```
Failed to clone langchain-ai/langchain: error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
error: 734 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2024-09-23 21:10_

Naively, this seems fine.

---

_Merged by @charliermarsh on 2024-09-23 22:09_

---

_Closed by @charliermarsh on 2024-09-23 22:09_

---

_Branch deleted on 2024-09-23 22:09_

---

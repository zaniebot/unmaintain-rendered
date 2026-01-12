```yaml
number: 3490
title: Remove Wasm-specific Rayon workarounds
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rayon
created_at: 2023-03-13T20:22:52Z
updated_at: 2023-03-13T20:48:45Z
url: https://github.com/astral-sh/ruff/pull/3490
synced_at: 2026-01-12T15:55:13Z
```

# Remove Wasm-specific Rayon workarounds

---

_@charliermarsh_

I believe Rayon added appropriate fallback behavior for non-thread-supporting environments in the most recent release (see https://github.com/rayon-rs/rayon/pull/1019), so this is no longer necessary.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-13 20:22_

---

_Review requested from @konstin by @charliermarsh on 2023-03-13 20:22_

---

_Comment by @github-actions[bot] on 2023-03-13 20:32_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@MichaReiser approved on 2023-03-13 20:48_

---

_Merged by @charliermarsh on 2023-03-13 20:48_

---

_Closed by @charliermarsh on 2023-03-13 20:48_

---

_Branch deleted on 2023-03-13 20:48_

---

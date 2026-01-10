```yaml
number: 14896
title: "ruff: add worktree support to build.rs"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/worktree-support
created_at: 2024-12-10T18:55:00Z
updated_at: 2024-12-10T21:23:23Z
url: https://github.com/astral-sh/ruff/pull/14896
synced_at: 2026-01-10T20:42:27Z
```

# ruff: add worktree support to build.rs

---

_Pull request opened by @BurntSushi on 2024-12-10 18:55_

Without this, `cargo insta test` re-compiles every time it is run, even
if there are no changes. With this, I can re-run `cargo insta test` (or
other `cargo build` commands) without it resulting in re-compiles.

I made an identical change to uv a while back:
https://github.com/astral-sh/uv/pull/6825


---

_Comment by @github-actions[bot] on 2024-12-10 19:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-12-10 19:05_

---

_Merged by @BurntSushi on 2024-12-10 19:06_

---

_Closed by @BurntSushi on 2024-12-10 19:06_

---

_Branch deleted on 2024-12-10 19:07_

---

_Label `internal` added by @MichaReiser on 2024-12-10 21:23_

---

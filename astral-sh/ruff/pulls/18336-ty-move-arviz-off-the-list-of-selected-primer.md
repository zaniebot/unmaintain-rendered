```yaml
number: 18336
title: "[ty] Move arviz off the list of selected primer projects"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/arviz-bad
created_at: 2025-05-27T16:43:42Z
updated_at: 2025-05-27T16:51:21Z
url: https://github.com/astral-sh/ruff/pull/18336
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Move arviz off the list of selected primer projects

---

_Pull request opened by @AlexWaygood on 2025-05-27 16:43_

## Summary

primer runs have been failing since https://github.com/arviz-devs/arviz/commit/3205b82bb4d6097c31f7334d7ac51a6de37002d0 landed on the arviz `main` branch. Seems like we're struggling with some of `xarray`'s type annotations? I confirmed locally that ty completes without crashing if you checkout the prior commit on the arviz `main` branch and then run `uv run --no-project --with=xarray ../ruff/target/debug/ty check`, but that it crashes with "too many cycle iterations" if you run the same command on https://github.com/arviz-devs/arviz/commit/3205b82bb4d6097c31f7334d7ac51a6de37002d0

## Test Plan

CI on this PR


---

_Review requested from @carljm by @AlexWaygood on 2025-05-27 16:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-27 16:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-27 16:43_

---

_Label `internal` added by @AlexWaygood on 2025-05-27 16:43_

---

_Label `ty` added by @AlexWaygood on 2025-05-27 16:43_

---

_Comment by @AlexWaygood on 2025-05-27 16:45_

Example crashes in CI:
- https://github.com/astral-sh/ruff/actions/runs/15280745284/job/42978865181
- https://github.com/astral-sh/ruff/actions/runs/15278735023/job/42971920972?pr=18333
- https://github.com/astral-sh/ruff/actions/runs/15278208171/job/42974577705?pr=18234

---

_Comment by @github-actions[bot] on 2025-05-27 16:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-27 16:50_

---

_Merged by @AlexWaygood on 2025-05-27 16:51_

---

_Closed by @AlexWaygood on 2025-05-27 16:51_

---

_Branch deleted on 2025-05-27 16:51_

---

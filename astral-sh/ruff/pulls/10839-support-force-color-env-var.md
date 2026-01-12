```yaml
number: 10839
title: Support FORCE_COLOR env var
type: pull_request
state: merged
author: carljm
labels:
  - cli
assignees: []
merged: true
base: main
head: cjm/force-color
created_at: 2024-04-08T21:03:18Z
updated_at: 2024-04-08T21:29:29Z
url: https://github.com/astral-sh/ruff/pull/10839
synced_at: 2026-01-12T15:55:33Z
```

# Support FORCE_COLOR env var

---

_@carljm_

Fixes #5499 

## Summary

Add support for `FORCE_COLOR` env var, as specified at https://force-color.org/

## Test Plan

I wrote an integration test for this, and then realized that can't work, since we use a dev-dependency on `colored` with the `no-color` feature to avoid ANSI color codes in test snapshots.

So this is just tested manually.

`cargo run --features test-rules -- check --no-cache --isolated - --select RUF901 --diff < /dev/null` shows a colored diff.
`cargo run --features test-rules -- check --no-cache --isolated - --select RUF901 --diff < /dev/null | less` does not have color, since we pipe it to `less`.
`FORCE_COLOR=1 cargo run --features test-rules -- check --no-cache --isolated - --select RUF901 --diff < /dev/null | less` does have color (after this diff), even though we pipe it to `less`.



---

_Label `cli` added by @carljm on 2024-04-08 21:03_

---

_Renamed from "Cjm/force color" to "Support FORCE_COLOR" by @carljm on 2024-04-08 21:04_

---

_Renamed from "Support FORCE_COLOR" to "Support FORCE_COLOR env var" by @carljm on 2024-04-08 21:05_

---

_Comment by @github-actions[bot] on 2024-04-08 21:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-08 21:27_

---

_Merged by @carljm on 2024-04-08 21:29_

---

_Closed by @carljm on 2024-04-08 21:29_

---

_Branch deleted on 2024-04-08 21:29_

---

```yaml
number: 21617
title: "Dogfood ty on the `scripts` directory"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/dogfood-ty
created_at: 2025-11-24T18:05:29Z
updated_at: 2025-11-24T23:13:46Z
url: https://github.com/astral-sh/ruff/pull/21617
synced_at: 2026-01-10T16:48:02Z
```

# Dogfood ty on the `scripts` directory

---

_Pull request opened by @AlexWaygood on 2025-11-24 18:05_

## Summary

This PR sets up CI jobs to run ty from the `main` branch on the files and subdirectories in our `scripts` directory

## Test Plan

Both these commands pass for me locally:
- `uv run --project=./scripts cargo run -p ty check --project=./scripts`
- `uv run --project=./scripts/ty_benchmark cargo run -p ty check --project=./scripts/ty_benchmark`

---

_Label `ci` added by @AlexWaygood on 2025-11-24 18:05_

---

_Label `ty` added by @AlexWaygood on 2025-11-24 18:05_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 18:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@AlexWaygood reviewed on 2025-11-24 18:15_

---

_Review comment by @AlexWaygood on `scripts/update_ambiguous_characters.py`:48 on 2025-11-24 18:15_

a nice little use of our feature where we allow redeclarations :-)

---

_Marked ready for review by @AlexWaygood on 2025-11-24 18:17_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-24 18:17_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-24 18:17_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-24 18:17_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-24 18:17_

---

_@amyreese approved on 2025-11-24 18:29_

---

_@carljm approved on 2025-11-24 22:29_

---

_Merged by @AlexWaygood on 2025-11-24 23:13_

---

_Closed by @AlexWaygood on 2025-11-24 23:13_

---

_Branch deleted on 2025-11-24 23:13_

---

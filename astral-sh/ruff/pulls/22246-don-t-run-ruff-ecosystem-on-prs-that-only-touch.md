```yaml
number: 22246
title: "Don't run ruff-ecosystem on PRs that only touch ruff_benchmark"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/benchmark-ecosystem
created_at: 2025-12-29T15:57:57Z
updated_at: 2025-12-29T17:33:55Z
url: https://github.com/astral-sh/ruff/pull/22246
synced_at: 2026-01-10T16:36:18Z
```

# Don't run ruff-ecosystem on PRs that only touch ruff_benchmark

---

_Pull request opened by @AlexWaygood on 2025-12-29 15:57_

## Summary

Currently we run the ruff-ecosystem CI job on any PR that touches the `ruff_benchmark` crate. This seems a bit silly, since it takes a while for this job to complete, and nowadays most commits touching the `ruff_benchmark` crate are just bumping assertions regarding the expected number of ty diagnostics.

This PR refines the logic in `ci.yaml` so that the ruff-ecosystem check only runs if code related to the linter or the formatter is changed. The benchmark job is also updated, so that the Ruff benchmarks run if code relating to the linter/formatter/parser/benchmarks changed, and the ty benchmarks run if code relating to the parser/type-checker/benchmarks changed.

The downside is that `ci.yaml` becomes even more complicated. We could consider splitting this workflow up into several workflow files to reduce its complexity.

## Test Plan

No idea.


---

_Label `ci` added by @AlexWaygood on 2025-12-29 15:57_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:08_


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

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:555 on 2025-12-29 16:28_

We should also run this job when the `ci.yaml` file changed or any file touching the ecosystem script

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:997 on 2025-12-29 16:29_

Isn't the parser included in ty's dependencies? If not, we should add it because we definitely want to run mypy primer on parser changes

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:1083 on 2025-12-29 16:29_

Same here, I would expect the parser to be in ty's deps

---

_@MichaReiser reviewed on 2025-12-29 16:30_

---

_@AlexWaygood reviewed on 2025-12-29 17:17_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:997 on 2025-12-29 17:17_

It is, yes. I added this condition just for safety, but I can get rid of it again if you find it confusing

---

_@MichaReiser reviewed on 2025-12-29 17:20_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:997 on 2025-12-29 17:20_

I'd prefer to remove it then

---

_@AlexWaygood reviewed on 2025-12-29 17:24_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:555 on 2025-12-29 17:24_

Both of those are already covered by `needs.determine_changes.outputs.linter` and `needs.determine_changes.outputs.formatter`

---

_@AlexWaygood reviewed on 2025-12-29 17:25_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:997 on 2025-12-29 17:25_

done

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-29 17:25_

---

_@MichaReiser approved on 2025-12-29 17:27_

---

_Merged by @AlexWaygood on 2025-12-29 17:33_

---

_Closed by @AlexWaygood on 2025-12-29 17:33_

---

_Branch deleted on 2025-12-29 17:33_

---

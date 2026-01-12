```yaml
number: 12867
title: Update OpenAI excluded notebooks from ecosystem checks
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
assignees: []
merged: true
base: main
head: dhruv/ecosystem-notebooks
created_at: 2024-08-13T16:33:48Z
updated_at: 2024-08-14T02:33:27Z
url: https://github.com/astral-sh/ruff/pull/12867
synced_at: 2026-01-12T15:55:42Z
```

# Update OpenAI excluded notebooks from ecosystem checks

---

_@dhruvmanila_

## Summary

Follow-up to #12864, we don't need to exclude these notebooks anymore.

## Test plan

- [x] Make sure that ecosystem checks are green.

---

_Label `ci` added by @dhruvmanila on 2024-08-13 16:33_

---

_Comment by @codspeed-hq[bot] on 2024-08-13 16:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/ecosystem-notebooks)

### Merging #12867 will **not alter performance**

<sub>Comparing <code>dhruv/ecosystem-notebooks</code> (cdc471c) with <code>main</code> (d0ac38f)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review requested from @carljm by @dhruvmanila on 2024-08-13 16:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-13 16:39_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-13 16:39_

---

_Comment by @github-actions[bot] on 2024-08-13 16:59_

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

_Comment by @dhruvmanila on 2024-08-13 17:02_

Um, this is confusing as I've checked it locally and there were no parse errors. Going to re-run.

---

_Comment by @dhruvmanila on 2024-08-13 17:27_

I verified that all 3 notebooks highlighted in the ecosystem checks are actually invalid because the cells are marked as "code" without any metadata and it contains plain text / markdown.

I'm not sure why there are no parse errors in the linter section.

---

_Renamed from "Remove `exclude` from `openai/openai-cookbook` in ecosystem" to "Update OpenAI excluded notebooks from ecosystem checks" by @dhruvmanila on 2024-08-13 17:28_

---

_@AlexWaygood approved on 2024-08-13 18:10_

---

_Merged by @dhruvmanila on 2024-08-14 02:33_

---

_Closed by @dhruvmanila on 2024-08-14 02:33_

---

_Branch deleted on 2024-08-14 02:33_

---
